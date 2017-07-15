
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
- 


[image-1]:	http://static.thomson.com/csapp/exceptions-physical-control-flow.png "physical control flow"