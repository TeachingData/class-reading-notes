# Shells
When I began my career the operating systems of the time where UNIX and Dos (Windows 3.1 was the new player on the block) so all computer users were expected to be well-versed in batch and shell commands. As hardware has improved (in cost and ability), GUIs have come to rule the computer and one's ability to use a **shell** has become less and less of a univeral need. **Except when dealing with Servers**, where my standard setup still looks like:

![basicUbuntuServer](/Image_Files/ServerSetup.png)

As \*nix systems remain the top choice in servers and networking, understanding their **shell** has remained an important factor for IT, IS, and Networking professionals (true of MS Server as well as the *ability to automate tasks with Powershell* is considered an essiential skill). As part of an Intro to Networking class, this article will overview the shell scripting commands most used in Powershell and bash (standard Linux Shell) - full coverage is provided in the "Scripting" course.

---
---

## What is Scripting?
Scripting is providing a set of commands to the shell for it to execute. **It is the glue that holds systems together.** These commands can be:
  - in the shell's language
  - executables built-in to the kernel
  - other programming languages (Python, C#, Perl)
  - sub-languages as part of an executable (awk, sed, grep)
  
The main idea behind creating a shell script is to either **lessen the load** of the client or **automate tasks** the system needs to perform. For example, to make an xml file more readable (in Powershell) we might:

    [xml] $info = Get-Content test.xml
    $info | Format-List *

While in most \*nix systems we'd use:
    
    tidy -xml -iq test.xml
    
Either of these could be a great help to a developer trying to understand some new XML file a client provided or be **piped** into another script for further parsing and automatic uploading to a database. 

---
---

## Basic Shell Scripting Commands
On Linux you can get more infomation using ***\<command \-\-help\>*** and in Powershell using ***Get-Help \<command\>***. The basic shell scripting commands are:

### Echo / Write-Host commands: Used for console display
    #bash
    echo "Hello World"

    #Powershell
    Write-Host "Hello World"

Note, if Echo/Write-Host is used by itself it will just print a blank line. We can also use it with a variable as follows (note $ placement):

    #bash
    test="Hello" # no $ in assignment
    echo "$test World"
    echo
    
    #Powershell
    $test = "Hello" # $ to assign
    Write-Host "$test World"
    Write-Host

### Cat: Show contents of a file
    # This is the same in bash/Powershell (aliased to Get-Content)
    cat filename.txt
Frequently, used with pipes to pass contents of file between cmdlets/executables or as quick check of file.

### Grep / Select-String: Search text for pattern
Grep (and egrep) can have their own class. Select-String is Powershell's "equiv" but its not quite equal. As a basic intro: it searches files for a provided string or pattern (Regex).

    #Search for any instance of "test in txt files"
    #bash (case-sensitive) use -i flag to ignore case
    grep "test" ./*.txt
    > This is a test
    > A test this is

    #PowerShell (not case-sensitive)
    Select-String -Path "./*.txt" -Pattern "test"

    > test.txt:1:This is a test
    > test.txt:2:A test this is
    > test.txt:3:Test Test Test

    #Use regex to only find test at start of line (last line)
    #bash (^ = start line in Regex)
    grep -i "^test" ./*.txt
    > Test Test Test

    # Powershell
    Select-String -Path "./*.txt" -Pattern "^test"
    > test.txt:3:Test Test Test

For more see [Digital Ocean's guide of Grep](https://www.digitalocean.com/community/tutorials/using-grep-regular-expressions-to-search-for-text-patterns-in-linux). 

### ls: List folder and files in directory
This command is the same in PowerShell or \*nix. It lists the folders and files present in a particular directory (either current or provided path). Powershell's **ls** is actually `ls -l` (long list: i.e. include details) in \*nix.

### pwd & cd: Find Path and Change Directory
So these are ran together often enough. cd changes your directory (**. = current directory, .. = parent directory**) and pwd prints it out. Again, these are the same in both Linux and Powershell and would look like:

    # Assuming in /Users
    cd .. #change to /
    pwd # would print /
    cd Users/Sam #change to this directory/sub-directory
    pwd # would print /Users/Sam

### Mkdir: Makes a directory
Same on both again: makes a directory with default permissions (and current user as owner). 

    cd /home
    mkdir Sam
    cd Sam
    pwd # would print /home/Sam

### CP, MV, Rename & Rename-Item: Basic file controls
As we are using a command-line: copy, move, and rename are a bit different. Copy is essentially the same (it makes a copy of the original file). However, move (think cut) deletes the original and makes a new copy of the file whereas rename just changes the file name. So many times you'll use ***mv*** over ***rename*** because you just want to recreate the file (you use rename to keep permissions and owner the same).

    # both systems
    cp ./test.txt ./test1.txt # make copy of file
    mv test.txt testing.txt #cut and paste file (original gone)
    
    # !!! NOTE: cp test1.txt test.txt would OVERWRITE the original without a warning
    # Also note above you can use path to specify or not (if in directory with file)
    
    # change name without effecting permissions/owner/group (second item cannot have path)
    # linux
    rename testing.txt test.txt 

    # PowerShell
    Rename-Item testing.txt test.txt
    # Can specify with: Rename-Item -Path "./testing.txt" -NewName "test.txt"

---
---
## Intermediate Commands

### sudo / su
No real powershell equivilent (can use *-RunAs* flag but its not the same). SU is just switch user - use it to change to different login. SUDO **runs a single command with admin permisions**. It doesn't have an equivilent on Windows (one can open Powershell "as admin") but you can use `-Verb runas` for a close approximation.

    #Linux
    sudo netstat -ab
    
    #Powershell as close as you can (it opens a new window)
    Start-Process netstat -ArgumentList "-ab" -Verb runas

### Netstat: Displays general network information
Can be used to display:
 - routing tables (can also use `route PRINT`)
 - network connections
 - masquerade connections
 - interface statistics
 - even more

### Nslookup: Displays the information of servers. 
Queries DNS and fetches information (ip address).

### Uptime / Get-Uptime: Gives the uptime of server
Whenever server is left unattended this can be queried (ran) to determine if any downtime happened. Easy to automate by just redirecting (see below) to a log which is periodically checked.

---
## Piping and Redirects
When it comes to automation, the ability to redirect output (pipe it) is essential. There are several variants on piping but it can be considered as the *couts, System.out, or other file operations* dealt with in most programming languages. The following are the main **pipes**.

### redirecting output
`>` is used to **redirect output** and ***write*** *it to a file*.

`>>` is used to **redirect output** and ***append*** *it to the end of a file*.

    # Linux
    echo "hello" > file.txt # this will overwrite the file
    echo " world" >> file.txt # this will just add

    # Powershell (using earlier grep)
    Select-String -Path "./*.txt" -Pattern "^test" > newthing.txt

In Powershell, you cannot redirect pure strings (literally `Write-Output or "Hello World"`). See Piping for how to perform this.

<!--
### redirecting input
`<` is used to **redirect input**.

    $ cat < file.txt

Output:

    hello


`<<` (called here document) is a **file literal or input stream literal**.

    $cat << EOF >> file.txt

Output:

    >

Here you can type whatever you want and it can be multiline. It ends when you type EOF (We used EOF in our example but you can use something else instead).

    > linux
    > is
    > EOF

Output:

    hello
    world!
    linux
    is

`<<<` (called here string) is the **same as `<<` but takes only one word**.

    $cat <<< great! >> file.txt

Output:

    hello
    world!
    linux
    is
    great!

Add this later
-->

### Piping ( | ): Pass the output received from one command to another command or script. 
So this is the scripting equivilent of *chaining*, that fun task where we passed information from one command to another (like a builder class: ```Build b = Build.create().withParameterA("A").withParameterB(2);```). In fact, with Powershell its the only way to redirect direct strings to files without having to deal with Array conversions using two cmdlets:

`Set-Content` is used to **redirect output** and ***write*** *it to a file*.

`Add-Content` is used to **redirect output** and ***append*** *it to the end of a file*.

    "Hello" | Set-Content -encoding UTF8 test.txt # we can also add encoding and should
    " World" | Add-Content -endoding UTF8 test.txt # only appended

You can also use them in automation tasks and parsing, such as finding all instances of *john 2ndword* in a file and then processing those instances to *categorize the word after john*.

    # -i = ignore case, -o = only words; xargs = make array into arguments
    # ./ = path to script (assumes shebang and executable permissions set)
    grep -io "john .*" ./*.txt | xargs ./parse_users.py
    ------------------------------------
    # Where parse_users.py contains
    ------------------------------------
    #!/usr/bin/python3.6
    import sys

    if __name__ == "__main__":
      for i, arg in enumerate(sys.argv):
        print(f"ARG: {i:>2}: {arg}")
---
---
# Summary
Scripting is a complex subject, and includes learning several languages, so though we cannot introduce a subject complete in such a small space. This should provide a basic overview of the main scripting commands and methods we will be using in (most of) your courses.
