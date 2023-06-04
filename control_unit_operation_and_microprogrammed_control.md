# Control Unit Operation and Microprogrammed Control
If we know the ISA, then we know the functions that the processor must perform. However, we also need to know the external interfaces, usually through a bus, and how interrupts are handled. The following list of things are needed to specify the function of the processor:
- Operations (opcodes)
- Addressing modes
- Registers
- I/O module interface
- Memory module interface
- Interrupts

## Micro-Operations
The sequence of instructions executed by the processor is not necessarily the same as the *written sequence*, which makes up the program, due to the existence of branching instructions. This "out-of-order" sequence is called the *time sequence*.

Furthermore, each instruction cycle consists of a number of smaller units. One convenient subdivision is fetch, indirect, execute, and interrupt, with only fetch and execute always occuring. These can be broken down further, into steps that only involve the processor registers, called **micro-operations**. These are the functional, or atomic, operations of the processor. 

### The Fetch Cycle
This causes an intruction to be fetched from memory. There are four registers involved:
- **Memory address register (MAR)**: connected to the address line of the system bus. Specifies the address in memory for a read or write operation
- **Memory buffer regsiter (MBR)**: connected to the data lines of the system bus. contains the value to be stored in memory or the last value read from memory.
- **Program counter (PC)**: holds the address of the next instruction to be fetched.
- **Instruction register (IR)**: holds the last instruction fetched.

At the beginning of the fetch cycle, the address of the next instruction is in the PC, the first step then is to move that address to the MAR. The control unit issues a READ command on the control bus, and the result appears on the data bus and is copied into the MBR. We also need to increment the PC by the instruction length to get ready for the next instruction. Since reading a word from memory, and incrementing the PC, do not interfere, they can be done simultaneously. The third step is to move the contents of the MBR to the IR, freeing up the MBR for an indirect cycle. Thus a simple fetch cycle consists of the following three steps:
```
    -  MAR <- (PC)
    -  MBR <- Memory ; PC <- (PC) + I
    -  IR <- (MBR)
```
where *I* is the instruction length.





