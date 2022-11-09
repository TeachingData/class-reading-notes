## Introduction
_Imagine a teenager going through a growth spurt_ - **hungry!** Their body requiring enormous amounts of food to make energy to fuel its exponental growth. 
This provides an idea of how much processes require memory.

### So what is memory on an OS? 
Though it takes many forms, the basics are the **RAM** provided by the operating systems (OS) and the **Solid State or Hard Disk Drives** installed[^1].
Unfortunetly for engineers, RAM itself is limited by the amount installed. Which can vary between 4Gb on small systems to [64Gb on gaming desktops](https://www.intel.com/content/www/us/en/gaming/resources/how-much-ram-gaming.html).
While the amount processes need varies from nearly nothing to Gbs (such as notepad requiring far less then Microsoft Word). Further, we must consider that most users 
will fork multiple applications (processes) simultaneously, and count the usage of each of these starving processes against the expensive, finite memory available.

Given this, we find the OS has two requirements:

  1.	Processes must be able to run until the user interrupts or terminates it (it shouldn't die due to lack of resources)
  2.	While maintaining point 1: we should not sacrifice performance to such a level that it is unusable or "slow"

Which, as students studying an OS, begs the questions of how the memory is being managed and what governs where the data from a process belongs in memory?

## Virtual Memory Overview
As with much of Software Engineering, this all boils down to **abstraction**. In this instance, Virtual memory works as an abstraction to _create the illusion of having infinite memory available_. To achieve this it avoids the limitation of RAM size by storing data on the disk drive when the RAM is getting full[^2]. This means:

> Virtual Memory = Swap Space (memory from hard drive) + Physical (RAM)

Swap memory is an excellent tool for storing unused (waiting, blocked, background) processes but load operations from a disk are orders of magnitude slower than RAM. So one should avoid using swapping when possible. To further cover this, we will look at swapping and another common method called Paging (shoutout to all my DB people whoo!).

## Swapping
Swapping is not virtual memory (in modern systems it will use virtual memory addresses so is highly related) but is simply a memory compresion technique which involves the transfer of a process (and all its threads) to secondary memory (disk) from primary (RAM) due to inactivity or low priority. 

![swapping between Swap Space and Main Memory](/Image_Files/swapping.png)

This has the benefit of allowing multiple, concurrent, processes to run but is less flexible due to the back and forth required by the swap. Its also slower due to this and the simple limitations of a disk drive (when compared to RAM access). Typically, its used with heavy workloads and to allow the CPU to access processes quickly (where the loss of speed is offset by the abiliity to complete large and/or numerous processes).

## Paging
Paging involves 2 parts:

1. Dividing known memory (RAM size) into **frames**[^3]
2. Dividing the logical memory of the OS into **fixed-sized units** called **pages**

Each of these are then stored in tables (a frame table and page table - confusing names I know). Virtual memory mappings are then made between these tables such that the pages will correspond to a frame (the physical memory address). A process which looks like (here with a fixed size of 3 for simplicity):

![virtual mapping of tables](/Image_Files/virtualmapping.png)

Note, that the memory needed (page table) is greater then the memory in system (frame table) which happens fairly often. This is the first part of how paging and 
virtual memory help to deal with memory limitations. We can keep assigning *"addresses"* as the system runs out of memory without having to force quit (interrupt) items running in the foreground (the priority). The programs don't really care if the addresses are virtual, only that it points to the resources they need when they run. This is why it is abstraction, the OS uses its scheduler (with its various algorithms) to set the state of processes while the program is happy to ignore the fact that it memory address isn't quite at a physical address yet as it knows the address it has will be when the system is ready. 

## Page-in, Page-out

While just being able to schedule items when we run out of ram is nice, its the ability to page in/out that is the heart of this technique. Let's first look at what would happen if we just manipulated the Physical Memory directly and removed a few processes. So assume I have the following running and its been loaded into our frames like:

![physical memory usage linear graph](/Image_Files/runningprograms.png)

This is nice and not much is wasted here but suppose we need to add a program (we want to open Python's IDLE), currently we cannot do this as we are completely full. With virtual memory, we can start the process, load it into the pages table, and then let the scheduler handle what program will be removed from memory to make room (likely one which is not in focus or background apps) - think of the spinning circle that your mouse cursor becomes when you open a new program. 

Why is this important? Well, let's say we hate virtual memory and want to handle it ourselves (besides we are done with notepad and have moved our files) so we close those two programs. We run into this problem:

![physical memory requires congruent slots](/Image_Files/runningprograms2.png)

We have enough memory to run it if we combine what Notepad and Files left but they are not sequential so there is no way to assign it to that without moving multiple processes (requiring every memory address of every function, dependency, variable, etc. to move). Using a virtual mapping (and our pages table) the process is much simplier:

![page out and page in](/Image_Files/virtualmapping2.png)

Here the mapping just changed from process 4 to process 2 due to "*something*" happening in the system that required this. This allows us to **page-out** process 4 (freezing its state in the pages table) then reuse the frames by assigning their mapping to process 2. Which should sound familar to the explainations in class, except now your beginning to see the internals.

## Conclusion
This is just meant as a quick introduction and concept model to learn about virtual memory, swapping, and paging. That makes it a bit of a cartoon model but a nesscary one because I could write a book on paging alone (and have for databases). The final concept to expain is this: **all of these are used by your OS**. Your OS is not a single algorithm or just the scheduler and dispatcher, it is everything we've covered in a 16 week course that barely scratches the surface. Which is really the magic of abstraction that an OS provides - we build and run our programs unaware of the complexity of the machine it is running on.

However, by pulling back that curtain and seeing the internal workings - one will become a better engineer. As knowing how something works, planning an improvement or better system, and then building it - is truly the heart of engineering.

----------

[^1]: Technically the memory hierarchy (minimum access time to maximum) would be: CPU Registers, SRAM (Cache), Main Memory (DRAM), Hard Disk Drive, Solid State Drive but we are trying to keep something simple here.
[^2]: Though related to swapping and paging operations, (and used interchangably by many) it should be noted that Virtual Memory typically refers to the 
use of virtual memory addresses as was covered in the modern OS portion of the lecture. This does depend on the OS as [Windows settings]([url](https://support.microsoft.com/en-us/windows/tips-to-improve-pc-performance-in-windows-b3b3ef5b-5953-fb6a-2528-4bbed82fba96#1)) (Performance Options, Advanced tab,
then Virtual memory area) directly references swapping.
[^3]: Unified Extensible Firmware Interface (UEFI), the BIOS, provides this information to the OS as part of its initialization process.
