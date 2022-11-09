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

The advantage of this is if we need to switch a process. Then our scheduler (using a standard algorithm) can determine which page needs to be paged out and which needs paged in. Freeing and then re-using the frames needed.
----------

[^1]: Technically the memory hierarchy (minimum access time to maximum) would be: CPU Registers, SRAM (Cache), Main Memory (DRAM), Hard Disk Drive, Solid State Drive but we are trying to keep something simple here.
[^2]: Though related to swapping and paging operations, (and used interchangably by many) it should be noted that Virtual Memory typically refers to the 
use of virtual memory addresses as was covered in the modern OS portion of the lecture. This does depend on the OS as [Windows settings]([url](https://support.microsoft.com/en-us/windows/tips-to-improve-pc-performance-in-windows-b3b3ef5b-5953-fb6a-2528-4bbed82fba96#1)) (Performance Options, Advanced tab,
then Virtual memory area) directly references swapping.
[^3]: Unified Extensible Firmware Interface (UEFI), the BIOS, provides this information to the OS as part of its initialization process.
