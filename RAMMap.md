# RAMMap

## Use Counts
Breakdown of physical RAM by page state and usage type
![Use Counts](/Images/rammap1.png)
### Page State
* **Active:** Pages already in a process working set, and non-pageable pages. Does not generate a page fault
* **Standby:** Pages removed from a process working set but not cleared - used as cache and generates a soft fault
* **Modified:** Same as standby but contents had changed. Used as cache and generates a soft fault, although must be written to disk before its cleared
* **Modified No Write:** Same as modified but does not need committing to disk on clearing
* **Transition:** Pages locked in memory by a driver
* **Zeroed:** Pages zeroed by zero page writer thread
* **Free:** Pages not allocated to any process and available for use. Will likely be zeroed unless system call to allocate it will end up overwriting e.g. process requesting a file loaded into RAM
* **Bad:** Pages marked as bad or faulty

### Usage Type
* **Process Private:** Private pages
* **Mapped File:** Shared pages that are backed by files on disk e.g. DLLs
* **Shareable:** Shared pages that can be used by more than one process
* **Page Table:** Pages used by the kernel to map virtual address space to backing RAM/PF/mapped files
* **Paged Pool:** Pages allocated by the kernel that *can* be paged to disk
* **Nonpaged Pool:** Pages allocated by the kernel that *cannot* be paged to disk
* **System PTE:** Pages used to store Page Table Entries
* **Session Private:** Pages used for sessions
* **Metafile:** Pages storing metafiles e.g. NTFS MFT
* **AWE:** Pages used by Address Windowing Extensions (always in RAM)
* **Driver Locked:** Non-pageable pages allocated by drivers e.g. balloon driver or Hyper-V
* **Kernel Stack:** Kernel thread stack pages
* **Unused:** Pages not in use - always either zeroed, free or bad
* **Large Page:** Parge page pages

## Processes
Breaks down physical RAM consumption on a per process, per page list basis
![Processes](/Images/rammap2.png)
* **Process:** Process image name
* **Session:** Session process is running in. Session 0 is system processes and services
* **PID:** Process ID
* **Private:** Private pages in the active page list
* **Standby:** Pages in standby page list
* **Modified:** Pages in modified page list
* **Page Table:** RAM consumed by kernel page table entries for process
* **Total:** Total physical RAM consumption

## Priority Summary
Shows physical RAM consumed by each priority in the standby page list. The repurposed column shows the sum size of pages that were repurposed for another process (so not soft faulted back into the working set).
![Priority Summary](/Images/rammap3.png)

## Physical Pages
Breaks down physical RAM consumption by individual page
![Physical Pages](/Images/rammap4.png)
* **Physical Address:** Page physical RAM address
* **List:** List the page is currently in
* **Use:** Usable type of page (see *Use Count* above)
* **Priority:** Priority of the page
* **Image:** Contains part of a image mapped file
* **Offset:** Page position in page file or mapped file
* **File Name:** Name of mapped file backing the page
* **Process:** Process owning private page
* **Virtual Address:** Virtual address of page if in active list and not a mapped file
* **Pool Tag:** Tag of paged/non-paged pool pages

## Physical Ranges
Shows ranged of physical RAM available - breaks in range normally taken up by drivers
![Physical Ranges](/Images/rammap5.png)

## File Summary
Lists every mapped file in RAM, and how much of each list it consumes
![File Summary](/Images/rammap6.png)
* **Path:** Path to mapped file
* **Total:** Total physical RAM consumed
* **Standby:** Space consumed in standby list
* **Modified:** Space consumed by modified list
* **Modified No-Write:** Space consumed in modified no-write list
* **Active:** Space comsumed by active list

## File Details
Lists further deatils of mapped files in RAM
![File Details](/Images/rammap7.png)
* **Path:** Path to mapped file
* **Size:** Total physical RAM consumed
* **Physical Address:** Physical address of page
* **List:** List page is in
* **Type:** Usage type (always mapped file)
* **Priority:** Priority of page
* **Image:** Indicates if page is executable
* **Offset:** Position of page in mapped file

# References
https://techcommunity.microsoft.com/t5/ask-the-performance-team/introduction-to-the-new-sysinternals-tool-rammap/ba-p/374717

Windows Internals, Part 1

Troubleshooting with the Windows Sysinternals Tools | Mark E. Russinovich & Aaron Margosis 