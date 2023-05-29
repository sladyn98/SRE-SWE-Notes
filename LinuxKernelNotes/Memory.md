

## Hardware
In simple terms, when a computer wants to access its memory, it can do so quickly with its registers or more slowly with its main memory. To speed up the process, modern CPUs have a cache, which is a fast memory that stores chunks of data from the main memory.

It's important to ensure that user processes in a computer only access the memory they are supposed to. This is done by using base and limit registers for each process. These registers define the valid range of memory that a process can access. If a process tries to access memory outside of its valid range, it will cause an error.

The operating system (OS) has access to all memory locations and manages the memory for different processes. It swaps code and data between the main memory and other storage devices like hard drives. Changing the contents of the base and limit registers is a privileged task only allowed for the OS kernel, which is the core part of the operating system.

#### Logical Address Space vs Physical Address Space
In simple terms, when a computer program runs, it uses logical addresses to refer to memory locations. These logical addresses are translated into physical addresses that the memory hardware actually recognizes.

Addresses that are determined during the compilation or loading of a program have the same logical and physical addresses. However, addresses created during program execution have different logical and physical addresses. In this case, the logical address is also called a virtual address.

The logical address space refers to the complete set of logical addresses used by a program, while the physical address space refers to the corresponding set of physical addresses.

The task of mapping logical addresses to physical addresses at runtime is handled by a component called the memory-management unit (MMU). The MMU can take various forms, but one common approach is to use a relocation register. This register's value is added to every memory request at the hardware level, effectively translating the logical addresses into physical addresses.


#### What is virtual memory and swapping and paging

Understand that `swap` is an extension of the address space of RAM i.e if the kernel requires more memory than it is available on the ram it will swap in and out of the swap space that is a special place on the hard disk.

Disk cache is just used to store disk memory that can be easily accessed, it borrows from the unused memory. If an app needs more memory it just deallocates from the disk cache.


<p align="center">
  <img src="images/memory_base.jpg" />
</p>

#### Dynamic Linking

Dynamic linking in DLLs offers several benefits compared to static linking. With static linking, each program that uses a routine from a library includes its own copy of that routine, wasting disk space and memory. In dynamic linking, only stubs are linked into the executable, referencing the actual library module at runtime. This saves disk space as only the stubs are included. If the library routines are reentrant (don't modify themselves), memory can be saved by loading a single copy of the routines into memory and sharing the code among processes. DLLs also facilitate easy upgrades and updates, as programs can be updated by loading new DLL versions without relinking. The stubs recognize and replace themselves with the actual routines on the first call, eliminating the stub overhead in subsequent calls. Dynamic linking enables efficient code reuse, reduced disk space usage, memory optimization, and easy updates.

## Contigous Memory Allocation


There are three major memory allocation algorithms

a) First Fit
b) Best Fit
c) Worst Fit

There are also two major fragmentation problems caused

a) Internal Fragmentation
b) External Fragmentation


Solution to external fragmentation: **Compaction**
To reduce external fragmentation, one approach is compaction. Compaction involves moving all the programs in memory to one end of the physical memory. By doing this, the gaps created by previously terminated programs are consolidated, creating larger blocks of contiguous memory that can be used for new programs. This process is done by updating a register called the relocation register for each program. The relocation register helps the computer translate logical addresses used by the program into physical addresses in memory.

## Pages

In summary, a logical address consists of a page number and an offset within that page. The page number limits the number of pages a process can address, while the offset determines the maximum size of each page. The page table maps the page number to a frame number in physical memory. The frame number determines the number of addressable frames, and the offset determines the size of each frame. For example, if the logical address size is 2^m and the page size is 2^n, the high-order m-n bits represent the page number, and the remaining n bits represent the offset. The number of bits for the page number and frame number can differ, as they relate to the logical address space and physical address space, respectively.

## Vritual Memory

**Page Tables and Page Faults**

 Page Table: A page table is an array of page table entries that map virtual pages to physical pages. The page table is managed by the operating system. 
 MMU: The memory management unit takes care of mapping the pages over the course of the instruction.
 Page Hit: A page hit occurs when CPU accesses a page that is present in the main memory or also known as DRAM.
 Page Fault: A page fault occurs when that piece of VM word is not present in the main memory and needs to be searched on the disk

 How are page faults handled: Page fault handler selects a victim to be evicted and grabs the actual data and puts it in main memory and then the instruction is restarted.

**Waiting until the miss to copy the page to DRAM is known as demand paging**

Zero page: The zero page is a section of memory in a computer system that contains memory addresses starting from zero. It is often used for memory optimization, storing pointers or addresses in a compact manner, and for critical system variables or functions. The smaller addresses in the zero page allow for faster access and can be utilized by system calls or interrupts. Its specific usage and significance can vary across different systems and operating systems.


<p align="center">
  <img src="images/page_table.jpg" />
</p>

a) The memory address requested is first checked, to make sure it was a valid memory request.
b) If the reference was invalid, the process is terminated. Otherwise, the page must be paged in.
c) A free frame is located, possibly from a free-frame list.
d) A disk operation is scheduled to bring in the necessary page from disk. ( This will usually block the process on an I/O wait, allowing some other process to use the CPU in the meantime. )
e) When the I/O operation is complete, the process's page table is updated with the new frame number, and the invalid bit is changed to indicate that this is now a valid page reference.
f) The instruction that caused the page fault must now be restarted from the beginning, ( as soon as this process gets another turn on the CPU. )
g) In an extreme case, NO pages are swapped in for a process until they are requested by page faults. This is known as pure demand paging.
h) In theory each instruction could generate multiple page faults. In practice this is very rare, due to locality of reference, covered in section 9.6.1.
i) The hardware necessary to support virtual memory is the same as for paging and swapping: A page table and secondary memory. ( Swap space, whose allocation is discussed in chapter 12. )
j) A crucial part of the process is that the instruction must be restarted from scratch once the desired page has been made available in memory. For most simple instructions this is not a major difficulty. However there are some architectures that allow a single instruction to modify a fairly large block of data, ( which may span a page boundary ), and if some of the data gets modified before the page fault occurs, this could cause problems. One solution is to access both ends of the block before executing the instruction, guaranteeing that the necessary pages get paged in before the instruction begins.


**Translation lookaside buffer**
the Translation Lookaside Buffer (TLB) is a hardware cache that stores recently accessed virtual-to-physical address translations. It helps speed up the translation process by providing quick access to translation information, reducing the need to access the main memory. TLB improves memory management performance in systems that use virtual memory

<p align="center">
  <img src="images/tlb.jpg" />
</p>