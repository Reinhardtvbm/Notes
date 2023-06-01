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

