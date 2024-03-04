# Table of Contents
This provides an overview of Bash scripting (and only Bash; see CreatingPSFiles for PowerShell) with the following sections:

  1. [Background Processes](#background-processes)
  2. [Parallelism](#parallelism)

---

# Background Processes
First a small definition reminder: A process is a running program. So whenever you hit run (Play Button) on your IDE you actually
start a process with your program but while your working on it (and not running it) - there's no process only the program.

Another example is how you're reading this on your browser and are able to click and interact with it - that is a **foreground** 
process. With GUI Desktops we are able to run many, many processes in the forground and background. Right now I'm running 4 items: 
Google Chrome (with many tabs open), Teams, Snip (to take this screenshot), and Task Manager and 115 background processes:

![image](https://github.com/TeachingData/class-reading-notes/assets/8454537/e2793885-17c9-44de-9011-940bb1ee5266)

When using terminal, running just one takes up all our time (though we can still run many, many background processes):

![image](https://github.com/TeachingData/class-reading-notes/assets/8454537/5eb92e2f-25fd-49fe-adfe-85e9a810ef80)

This means, in order to work efficiently and quickly enough to properly monitor our system we have to understand how to
fork and create **background processes**. Bash allows this using the '&' character like:

![image](https://github.com/TeachingData/class-reading-notes/assets/8454537/576aeedd-281b-4450-bc7b-aa3a5ddda089)

Adding an & after a command tells the shell to run it in the background. Note, if you don't redirect it the script 
will start printing whenever it gets done which is likely not what you wantn (so redirect after the background prompt). 
We can check what processes are running with the  `ps` command and stop the using `kill pid#`, such as:

![image](https://github.com/TeachingData/class-reading-notes/assets/8454537/cbde639b-0ef4-4b78-a2c8-170c920e40bb)

This leads to some interesting possibilities, including how we can use it with items like netstat, curl, nmap, and other tools 
for logging and data mining. To see more on the commands used check `man kill ; man ps ; man sleep`.

# Parallelism
So to build bots for everything I meantioned in part 1 - we need to setup our processes to run in parallel. So first let's look 
at a known dead link and see how curl allows us to find if its bad or good. So the "known" bad link is [https://stackoverflow.com/questions/29044687/example-projects-in-c](https://stackoverflow.com/questions/29044687/example-projects-in-c) 
(cause you can only see it with a high rep at StackOverflow and we didn't add any credentials to curl):

![image](https://github.com/TeachingData/class-reading-notes/assets/8454537/30cd2d1a-b836-47be-a72f-e8f1abf5b928)

So [HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) show us if a link is bad or good and for 1 item just running curl is fine.
Wikipedia has [6,790,815 articles at the time of this writing](https://en.wikipedia.org/wiki/Wikipedia:Size_of_Wikipedia) so 1 at a time is just not going to cut it
whether in the background or foreground. So now we need to change the script and instead use this psuedocode:

    
    # this is psuedocode I haven't tested it
    function(file):
      while IFS= read -r line; do
         curl -I $line
         # Because now we'd have tons of checking each link
         # use grep, split the lines with -r, other parsing and redirect output to file here
      done < $file

      function &

The code is part of [the standard Bash FAQ](https://mywiki.wooledge.org/BashFAQ/001) so I'll let you read that for now (we cover it more a section or two down), 
but there are a few things to note: 

  1. we are running the processes in parallel because the background processes will all fork off of the main process
  2. we had to put the code in a function to run it all as a single step to ensure full processing
  3. if we open a file for writing, we run the risk of having locking issues

Let's look at each of those issues now.....*see full book*



