
# Exception Control Flow

## Control Flow
> Processors do only one thing:
- From startup to shutdown, a CPU simply reads and executes(interprets) a sequence of instructions, one at a time.
- This sequence is the CPU’s **control flow (or flow of control)**
![][image-1]

## Altering the Control Flow
There are tow mechanisms for changing control flow, both react to changes in **Program state**:
- Jumps and branches
- Calls and return

But this is insufficient for a useful system, it’s difficult to react to changes in **System state**:
- data arrives from a disk or a network adapter
- instruction divides by zero
- user hits `Ctrl-C` at the keyboard
- system timer expires

> **System needs mechanisms for “exceptional control flow”**

## Exceptional Control Flow
- Exists at all levels of a computer system
- Low level mechanisms
	- Exceptions: change in control flow in response to s system event(i.e., change in system state)
	- Combination of hardware and OS software
- Higher level mechanisms
	- Process context switch
	- Signals
	- Nonlocal jumps: `setjmp()`/`longjmp()`
	- Implemented by either:
		- OS software (context switch and signals)
		- C language runtime library (nonlocal jumps)

## Exceptions
- An exception is a transfer of control to the OS in response to some event (i.e., change in processor state)
![][image-2]

- Examples: div by 0, arithmetic overflow, page fault, I/O request completes, Ctrl-C

### Exception Handing
> Exceptions can be difficult to understand because handing then involves close cooperation between hardware and software.

![][image-3]

- Each type of event has a unique exception number `K`
- `K` = index into exception table
- Handler K is called each time exception k occurs

### Classes of Exceptions
Exception can be divided into four classes: **interrupts, traps, faults, ** and **aborts**.

#### Asynchronous Exceptions (Interrupts)
- **Interrupts**: interrupts occur asynchronously as a result of signals from I/O devices that are external to the processor. 
	- Caused by events external to the processor
	- indicated by setting the processor’s interrupt pin
	- Handler returns to “next” instruction
- Examples:
	- I/O interrupts
		- hitting `Ctrl-C` at the keyboard
		- arrival of a packet from a network
		- arrival of data from a disk
	- Hard reset interrupt
		- hitting the reset button
	- Soft reset interrupt
		- hitting `Ctrl-Alt-Delete` on a PC



[image-1]:	http://static.thomson.com/csapp/exceptions-physical-control-flow.png "physical control flow"
[image-2]:	http://static.thomson.com/csapp/exceptions-anatomy-of-an-exception.png
[image-3]:	http://static.thomson.com/csapp/exceptions-exception-handling.png