
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

[image-1]:	http://static.thomson.com/csapp/physical-addressing.png