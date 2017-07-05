
## 术语卡：Virtual Memory
> **Virtual Memory** is an elegant interaction of hardware exceptions, hardware address translation, main memory, disk files, and kernel software that **provides each process with a large, uniform, and private address space**.

### 印象
**VM** provides three important capabilities:
- It uses main memory efficiently by treating it as **a cache for an address space stored on disk, keeping only the active areas in main memory, and transferring data back and forth between disk and memory as needed**.
- It simplifies memory management by **providing each process with an uniform address space**.
- It protects the address space of each process from corruption by other processes.

## Physical and  Virtual Addressing
- **Physical Addressing**:
![][image-1]
> Used in “simple” systems like embedded micro-controllers in devices like cars, elevators, and digital picture frames.

- **Virtual Addressing**:
![][image-2]
> - Used in all modern servers, desktops, and laptops
> - One of the great ideas in computer science
> - **Address translation**: the task of converting a virtual address to a physical one. Dedicated hardware on the CPU chip called the `memory management unit(MMU)` translates virtual addresses on the fly, using a look-up table stored in main memory whose contents are managed by the operating system.

## Address Spaces
- **Linear address space**: Ordered set of contiguous non-negative integer addresses: 
	`{0, 1, 2, ...}`
- **Virtual address space**: In a system with virtual memory, the CPU generates virtual addresses from an address space of `N = 2^n` addresses.
	`{0, 1, 2, ..., N-1}`
- **Physical address space**: A system also has physical address space that corresponds to the `M` bytes of physical memory in the system:
	`{0, 1, 2, ..., M-1}`

> - Clean distinction between data (bytes) and their attributes (address)
> - Each object can now have multiple addresses
> - Every byte in main memory: **one physical address, one (or more) virtual address**

### Why Virtual Memory?
- **Uses main memory efficiently**: use DRAM as a cache for the parts of a virtual address space
- **Simplifies memory management**: Each process gets the same uniform linear address space
- **Isolates address space**: 
	- one process can’t interfere with another’s memory
	- User program cannot access privileged kernel information

## VM as a Tool for Caching
- **Virtual memory** is an array of N contiguous bytes stored on disk.
- The contents of the array on disk are cached in **physical memory(DRAM cache)**.
	- These cache blocks are called **pages (size is P = 2^p bytes)**
	> VM systems handle this by partitioning the virtual memory into fixed-sized blocks called **virtual pages (VPs)**. Each virtual page is P=2^p bytes in size. Similarly, physical memory is partitioned into **physical pages (PPs)**, also P bytes in size. (Physical pages are also referred to as page frames.)
![][image-3]
Three disjoint subsets of virtual pages:
- **Unallocated**: Pages that have not yet been allocated (or created) by the VM system.
- **Uncached**: Allocated pages that are not cached in physical memory.
- **Cached**: Allocated pages that are currently cached in physical memory.

### DRAM Cache Organization
> the bottom line is that the organization of the DRAM cache is driven entirely by the enormous cost of misses.(归根结底，DRAM缓存的组织结构完全是由巨大的不命中开销驱动的）
- DRAM cache organization driven by the enormous miss penalty: 
	- DRAM is about `10x` slower than SRAM(the L1, L2 and L3 cache)
	- Disk is about `10,000x` slower than DRAM

- Consequences
	- Large page (block) size: typically `4-8 KB`, sometimes `4MB`
	- Fully associative:
		- Any VP can be placed in any PP
		- Requires a “large” mapping function - different from CPU caches
	- Highly sophisticated, expensive replacement algorithms
		- Too complicated and open-ended to be implemented in hardware
	- Write-back rather than write-through （总是使用写回，而不是直接写）


[image-1]:	http://static.thomson.com/csapp/vm-physical-addressing.png
[image-2]:	http://static.thomson.com/csapp/vm-virtual-addressing.png
[image-3]:	http://static.thomson.com/csapp/vm-virtual-pages-physical-pages.png