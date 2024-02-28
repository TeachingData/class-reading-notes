# Table of Contents
This provides an overview of Bash scripting (and only Bash; see CreatingPSFiles for PowerShell) with the following sections:

  1. Text Editors vs. IDEs
  2. Variables in Bash
  3. Controling the flow (if/else)
  4. Looping (while & for)
  5. Functions

---
---

# Text Editor vs. IDE

Integrated Development Environments include products like Jupyter Notebooks, IntelliJ, Eclipse, Android Studio, XCode, PyCharm, and many more. These are wonderful tools and work by bundling all your development needs into a single package whose sole focus is building programs in a specific language (or languages). This allows it to add:
  - built-in debuggers
  - Code Completion and Syntax highlighting
  - Version Controls
  - Refactoring tools
by default. However, adding these features carries a lot of weight (both in storage and processing needs).

Text Editors include Notepad, Nano, VIM, EMACs, and VStudioCode (among others) and let you write and save text..... Yeah, that's it by default though some of the features are available through flags and options. For instance with nano we'll use something like the following:

    nano --syntax=sh dircounts.sh
    
The flag `--syntax` tells nano to open the file with syntax highlighting for shell. We'll look at other features later but for now its good to get used to not having code complete and, as we are learning shell, we need to learn about stderr a bit so the debugger is not missed and we'll be learning git so we'll provide our own version control later.
  
# Variables

![image](https://user-images.githubusercontent.com/8454537/203440541-88a9f8f2-78e2-4b23-be8f-b3cd32cfc8fb.png)

