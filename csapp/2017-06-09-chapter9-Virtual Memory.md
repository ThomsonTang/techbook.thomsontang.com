
## 术语卡：Virtual Memory
> **Virtual Memory** is an elegant interaction of hardware exceptions, hardware address translation, main memory, disk files, and kernel software that **provides each process with a large, uniform, and private address space**.

### 印象
**VM** provides three important capabilities:
- It uses main memory efficiently by treating it as **a cache for an address space stored on disk, keeping only the active areas in main memory, and transferring data back and forth between disk and memory as needed**.
- It simplifies memory management by **providing each process with an uniform address space**.
- It protects the address space of each process from corruption by other processes.

# Physical and  