## Introduction
_Imagine a teenager going through a growth spurt_ - **hungry!** Their body requiring enormous amounts of food to make energy to fuel its exponental growth. 
This provides an idea of how much processes require memory.

### So what is memory on an OS? 
Though it takes many forms, the basics are the **RAM** provided by the operating systems (OS) and the **Solid State or Hard Disk Drives** installed.
Unfortunetly for engineers, RAM itself is limited by the amount installed. Which can vary between 4Gb on small systems to [64Gb on gaming desktops](https://www.intel.com/content/www/us/en/gaming/resources/how-much-ram-gaming.html).
While the amount processes need varies from nearly nothing to Gbs (such as notepad requiring far less then Microsoft Word). Further, we must consider that most users 
will fork multiple applications (processes) simultaneously, and count the usage of each of these starving processes against the expensive, finite memory available.

Given this, we find the OS has two requirements:
  1.	That the software always runs until user aborts it, i.e. it should not auto-abort because OS has run out of memory.
  2.	The above activity, while maintaining a respectable performance for the softwares running.

Which, as students studying an OS, the questions of how the memory is being managed and what governs where the data from a process belongs in memory?

## Physical Memory Issues

## Virtual Memory
### Overview
As with much of Software Engineering, this all boils down to **abstraction**. In this instance, Virtual memory works as an abstraction to _create the illusion of having 
infinite memory available_. To achieve this it avoids the limitation of RAM size by storing data on the disk drive when the RAM is getting full[^1]. This means:

> Virtual Memory = Swap Space (memory from hard drive) + Physical (RAM)

Swap memory is an excellent tool for storing unused (waiting, blocked, background) processes but load operations from a disk are orders of magnitude slower than RAM. So
one should avoid using swapping when possible.

## Paging vs. Synchronization Overview

## Synchronization

## Paging

----------

[^1]: Though related to swapping and paging operations, (and used interchangably by many) it should be noted that Virtual Memory typically refers to the 
use of virtual memory addresses. This does depend on the OS as [Windows settings]([url](https://support.microsoft.com/en-us/windows/tips-to-improve-pc-performance-in-windows-b3b3ef5b-5953-fb6a-2528-4bbed82fba96#1)) 
(Performance Options, Advanced tab, then Virtual memory area) directly references swapping.
