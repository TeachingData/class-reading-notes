# Table of Contents
This provides an overview of Bash scripting (and only Bash; see CreatingPSFiles for PowerShell) with the following sections:

  1. [Shell Scripts with Text Editors vs. IDEs](#text-editor-vs-ide)
  2. Variables in Bash
  3. Looping (while & for)
  4. Controling the flow (if/else)
  5. Functions

---
---

# Text Editor vs. IDE

All the tasks so far have been focused on just executing commands in the shell and then using pipe to chain them together (with a redirect to a file). Though useful, most scenarios require more control and the ability to perform a series of commands - for this we have shell scripts. Scripting langauges all have their own variables, control flows, loops, functions, and syntax (as any programming language will). 

However, what makes shell scripting different from other programming languages is that it is optimized for performing shell-related tasks. Thus, creating command pipelines, saving results into files, and reading from standard input are considered the true primitives in the same way that int, bool, and double are primitives. This makes it perfect for our purposes but also adds a bit of weird-ness if you're not used to it. For this section, we will look at the first one you encounter - using a text editor instead of an Integrated Development Environment (IDE).

## Integrated Development Environments 
IDEs include products like Jupyter Notebooks, IntelliJ, Eclipse, Android Studio, XCode, PyCharm, and many more. These are wonderful tools and work by bundling all your development needs into a single package whose sole focus is building programs in a specific language (or languages). This allows it to add:
  - built-in debuggers
  - Code Completion and Syntax highlighting
  - Version Controls
  - Refactoring tools
by default. However, adding these features carries a lot of weight (both in storage and processing needs).

## Using a Text Editors with syntax highlighting
Text Editors include Notepad, Nano, VIM, EMACs, and VStudioCode (among others) and let you write and save text..... Yeah, that's it by default though some of the features are available through flags and options. For instance with nano we'll use something like the following:

    nano --syntax=sh dircounts.sh
    
The flag `--syntax` tells nano to open the file with syntax highlighting for shell. We'll look at other features later but for now its good to get used to not having code complete and, as we are learning shell, we need to learn about stderr a bit so the debugger is not missed and we'll be learning git so we'll provide our own version control later.
  
# Variables
We'll split this into 3 sections: using variables for literals, variables for command results, and calculations with variables. Meaning we'll start with the simple and move to the more complicated.

## Setting and printing variables with literals
Bash is a dynamic typed language (like Python or JavaScript). Meaning the following is perfectly fine:

    # this is a comment (use # to start one) and we're going to set variable foo to bar
    # Note: there is no spaces between the assignment operator (=) and the variable name or values
    foo="bar"

    # to print the variable you use echo and a dollar sign ($) which tell it to go grab the value currently assigned (so this will display "bar")
    echo $foo
    
    # Now in static typed languages you'd be stuck with a string but we can set it to THE ANSWER (to life, the universe, and everything)
    foo=42
    echo $foo

Pretty simple right? Try a few simple values to set and print but remember when I said primitives feel different in shell? Whelp, next let's look at how different it is.

## Variables for command results
Bash (and most shell scripts) use a lot of commands within themselves. As such, you'll end up wanting to store the results inside variables very often. As an example, let's look at ls (list) with its -A flag (almost all so all files but . & ..) and wc (word count) with the --words flag as using both allows us to see how many files are in a directory.

    # so the command with pipe is ls -A | wc --words to capture that we'll use the special variable designator $()
    count=$(ls -A | wc --words)
    echo $count

The `$()` is called the command substitution operator and allows us to run any shell command between the parens () and get the output for later use. You can also just use it to run a command with redirect without assigning to a variable. Such as, if we wanted to log that file count we'd run the following instead:

    # Run same command but don't store in a variable, rather we'll output it (append) to a file
    $(ls -A | wc --words >> directory_count.log)

Now this is useful but what's even better is using a combo! So let's say we want to have a variable that holds a directory we want to check? We can combine our command substitution with a standard variable for that:

    check="/home/kali"
    count=$(ls -A $check | wc --words)
    # count will now hold a count of all files in the home directory of kali (default login for kali Linux)

This is great but leaves one small issue: what if I want to do some calculations with that number? That takes a bit of a different approach.

## Variables and Calculations
So before we get back to our early example - let's look at just a simple calculation like 8-7 : should give us 1, right? Try it and it won't work: `echo 8-7` prints *"8-7"*, even if you try to assign it to a variable it won't work:

    simple=8-7
    echo $simple
    # this prints 8-7

So what's the issue? You have to tell shell that your expecting a binary operation (math or logical comparison) which you do with double parens `(( ))`. How does it work? Let's see:

    ((simple=8-7))
    echo $simple
    # Now this works

Note the simple variable is assigned inside the double parens. If you put it outside you'll get a parameter syntax error because Bash is looking for the internal function or the inside parens then expanding that to the calculation using the outer ones (the function is assignment in this case). You can also ignore the variable but only by telling Bash that we need to evaluate (grab the value its creating) with our good friend: $

    # We will just run the calcuation inside echo so we need to use $
    echo $((8-7))
    # Run the above without the $ and you'll get another syntax error

We'll see more about double parens (and even parameter expansion) as we continue with scripting but for now - that covers variables. Next we use them with for & while loops.
