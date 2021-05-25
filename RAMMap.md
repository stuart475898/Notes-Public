# RAMMap

## Use Counts
Breakdown of physical RAM by page state and usage type
![Use Counts](/Images/rammap1.png)
### Page State
* **Active:** Pages already in a processes working set, and non-pagable pages. Does not generate a page fault
* **Standby:** Pages removed from a processes working set but not cleared - used as cache and generates a soft fault
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
* **Page Table:** Pages used by the kernal to map virtual address space to backing RAM/PF/mapped files
* **Paged Pool:** Pages allocated by the kernel that *can* be paged to disk
* **Nonpaged Pool:** Pages allocated by the kernel that *cannot* be paged to disk
* **System PTE:** Pages used to store Page Table Entries
* **Session Private:** Pages used for sessions
* **Metafile:** Pages storing metafiles e.g. NTFS MFT
* **AWE:** Pages used by Address Windowing Extensions (always in RAM)
* **Driver Locked:** Non-pageable pages allocated by drivers e.g. ballon driver or Hyper-V
* **Kernel Stack:** Kernel thread stack pages
* **Unused:** Pages not in use - always either zeroed, free or bad
* **Large Page:** Parge page pages
