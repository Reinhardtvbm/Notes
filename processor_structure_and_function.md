# Processor Structure and Function 
## Processor Organisation
The main requirements of a processor are:
- **fetch instruction**: read an instruction from memory
- **interpret instruction**: the intrstruction is decoded to determine the action required
- **fetch data**: read data from memory or an I/O module
- **process data**: perform arithmetic and logic operations on data
- **write data**: write data that results from execution to memory or I/O modules

To do these, it is clear the the processor requires internal memory to store data temporarily. It also requires an ALU for the data processing, and a *control unit* to control the movement of data and instructions into and out of the processor, as well as the operation of the ALU.

## Register Organisation
Registers in the processor perform two main roles:
- **user-visible registers**: allow the programmer to minimise memory references by optimising the use of registers.
- **control and status registers**: used by the control unit, and by privelged OS programs to control the execution of programs

### User-Visible Registers
These registers *can be referenced by machine language*. There are four categories:

#### General Purpose
can be assigned many functions by the programmer. They can contain any *operand* for any *opcode*. Sometimes there are limitations, like dedicated registers for floating-point or stack operations. Sometimes they can be used for addresses. 

#### Data
Hold data

#### Address
These may be somewhat general purpose. Examples include:
- **segment pointers**: with segmented addressing, a segment register contains the base of the segment
- **index registers**: used for index addressing
- **stack pointer**: if there is user-visible stack addressing, then typically there is a dedicated regsiter that points to the top of the stack

#### Condition Codes
These are the least visible to the user. Also called *flags*, are bits set in the hardware as a result of operations. These codes are collected into one or more registers. The programmer cannot alter these bits. 

### Control and Status Registers
Four essential control and status registers:
- **program counter(PC)**: address of next instruction to be fetched
- **instruction register (IR)**: contains the instruction most recently fetched
- **memory address register (MAR)**: address of a location in memory 
- **memory buffer register (MBR)**: contains a word of data that is to be written to memory or has been most recently read from memory 

These registers are used for movement of data between the processor and memory. 

Many processors also include a register called the *program status word* (PSW), which typically contains condition codes and other status information:
- **sign**: the sign bit of the last arithmetic operation
- **zero**: set when the result is zero
- **carry**: set if the operation resulted in a carry
- **overflow**: indicates arithmetic overflow
- **interrupt enable/disable**: used to enable/disable interrupts
- **supervisor**: indicates whether the program is running in supervisor or user mode. some priveleged instructions can only be executed in supervisor mode.

Control information is often times specifically included in the design for operating system support. It is common to dedicate the first few hundred or thousand words of memory for control purposes.


## Instruction Cycle
An instruction cycle includes the following stages:
- **fetch**: read the next instruction into the processor
- **execute**: interpred the opcode and perform the indicated operation
- **interrupt**: if there is an interrupt, the current state must be saved, and then service the interrupt

## Instruction Pipelining
### Pipelining Strategy
Pipelining exploits the fact that an instruction consists of a number of stages. As a simple example consider the fetch and execute stages. There are times during execute that main memory is not being accessed, so we can fetch the next instruction in parallel to the execute stage. This is called *pre fetch* or *fetch overlap*.

This has the potential of doubling the speed of execution, however this outcome is unlikely for the following reasons:
- time to execute is generally longer than the time to execute, thus the fetch stage may have to wait for some time before it can empty its buffer
- a conditional branch instruction makes the address of the next instruction to fetch unknown. So the fetch stage must wait until execute is done for these instructions
