# Virtual Address Space
* Each process will get its own virtual address space
* Virtual address space addresses are mapped to physical address spaces via a page table
* Virtual address space can be backed by
  * Physical RAM
  * Page files
  * Mapped files
* As each process has its own address space managed by the memory manager, processes can use the same addresses
* A process can have more virtual address space allocated to it than available RAM

###### References
* https://docs.microsoft.com/en-us/windows-hardware/drivers/gettingstarted/virtual-address-spaces
* https://docs.microsoft.com/en-us/windows/win32/memory/virtual-address-space
* https://en.wikipedia.org/wiki/Virtual_address_space

# Working Set
* A process working set is the amount of virtual address space backed by physical RAM
* Working set is comprised of:
  * **Private WS:** Pages that are private to the process and cannot be access by other processes (except those holding SeDebugPrivilege)
  * **Shared WS:** Pages that are shared between processes e.g. DLLs
* Only pageable pages are included in the working set value - non-pageable pages such as AWE and large pages are not
* Processes are allocated a minimum working set size (guaranteed RAM backed pages) and a maximum working set size (maximum RAM backed pages)

###### References
* https://docs.microsoft.com/en-us/windows/win32/procthread/process-working-set
* https://docs.microsoft.com/en-us/windows/win32/memory/working-set

# Shared Memory/Mapped Files
* A process virtual address space can be mapped to portions of files on disk e.g. DLLs
* These can be shared between processes safely as the memory manager implements copy-on-write for these pages
  * If a process wants to modify a shared page, it copies those pages to be changed to another location, updates the writing process page table to map its virtual address space to the new physical RAM addresses and then carried out the write

###### References
* https://docs.microsoft.com/en-us/windows/win32/memory/file-mapping


# Virtual Address Space Page States
* A processes pages can be in one of the following states:
  * **Committed:** The memory manager will allocate pages from both RAM and disk (as required) to the process
  * **Reserved:** The memory manager will allocate pages in the virtual address space, but they wont be backed by physical RAM or disk yet (if later committed when not enough RAM/PF available, commit with fail). Generally used when processes know they need some contiguous virtual address space later but not now
  * **Free:** Virtual address space that is not in use (not committed or reserved)

###### References
* https://docs.microsoft.com/en-us/windows/win32/memory/page-state
* https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualalloc
* https://docs.microsoft.com/en-us/windows/win32/api/memoryapi/nf-memoryapi-virtualfree

# Physical RAM Page States
* Physical RAM pages can be in the following states:
  * **Zero Page:** A page that has been zeroed by the zero page thread and ready for allocation to a process
  * **Free Page:** A page that has yet to be zeroed - typically pages never allocated or pages that were allocated but no returned to the memory manager for use else where
  * **Standby Page:** A page no longer used by any process but still holds data - typically from processes that have exited. Used as cache e.g. if it held DLLs these can be kept here for use by a future process, so long as there isn't memory contention
  * **Modified Page:** A page removed from a working set because it hasn't been recently used, but has yet to be committed to disk. Can be put back into the working set, or written to disk if needed else where
  * **Active Page (Working Set):** A page actively being used by a process in RAM
  
###### References
* https://answers.microsoft.com/en-us/windows/forum/windows_10-performance/physical-and-virtual-memory-in-windows-10/e36fb5bc-9ac8-49af-951c-e7d39b979938

# Page Faults
* Page faults occur when a process attempts to read a part of its virtual address space but there is some issue
* Some page faults are benign and expected, while others can cause a BSOD
  * **Hard page fault:** Occurs when a page is stored in the page file or mapped file
  * **Soft page fault:** Occurs when a page is already in physical RAM somewhere, but isn't currently mapped to a process virtual address space
  
##References
* https://docs.microsoft.com/en-us/windows/win32/memory/working-set



