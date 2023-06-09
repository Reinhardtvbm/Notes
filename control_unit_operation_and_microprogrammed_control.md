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
where *I* is the instruction length, and each step requires one clock cycle. Not that the PC increment could have been grouped with the third step as well. The groupings of microinstructions follow two simple rules:
1. The proper sequence of events must be followed.
2. Conflicts must be avoided. One cannot read to and write from the same register in one time unit. ex. (MBR <- Memory) cannot be grouped with (IR <- MBR).

### The Indirect Cycle
Next we need to source operands. If the instruction specifies an indirect address, then an indirect cycle must precede the execute cycle. The data flow includes the following micro-instructions:
```
    -  MAR <- (IR(Address))
    -  MBR <- Memory
    -  IR(Address) <- (MBR(Address))
```
The address field of the IR is moved to the MAR, then the MBR is loaded with the word from memory. Finally, the IR is updated from the MBR. So now it contains a direct address. Now it is ready for the execute cycle

### The Interrupt Cycle
At the completion of the execute cycle, a test is made to determine whether any enabled interrupts have occurred. If so, the interrupt cycle occurrs. We have
```
    -  MBR <- (PC)
    -  MAR <- Save_Address ; PC <- Routine_Address
    -  Memory <- (MBR)
```
The MBR gets the contents of the PC so they can be saved for return from interrupt. Then the MAR is loaded with the address at which the contents of the PC are to be saved, and the PC is loaded with address of the start of the interrupt-processing routine. Then we save the MBR to memory. 

### The Execute Cycle
The execute cycle is not so predictable. There are many opcodes, therefore there are many different sequences of microinstructions that can occur. The control unit examines the opcode and generates a sequence of micro-operations based on the value of the opcode. This is referred to as instruction decoding. consider `ADD R1, X`:
```
    -  MAR <- (IR(address))
    -  MBR <- Memory
    -  R1  <- (R1) + (MBR)
```

### The Instruction Cycle
Now we need to tie the sequence of micro-instructions together. We use an instruction cycle code (ICC) which represents one of the four cycles: fetch, indirect, execute and interrupt. The ICC is set at the end of each cycle.

## Control of the Processor
### Functional Requirements
Now that the functioning of the processor has been decomposed into its most elementary components, we can define exactly what it is that the control unit must cause to happen. Thus we can define the functional requirements of the control unit. Those functions that the control unit must perform. The following two tasks are performed by the control unit:
- **Sequencing**: The control unit causes the processor to step through a series of micro-operations in the proper sequence, based on the program being executed.
- **Execution**: The control unit causes each micro-operation to be performed.
The key to this is control signals

### Control Signals
The control unit has the following input signals:
- **clock**
- **IR**
- **Flags**
- **Control signals from the control bus**

and it has the following outputs:
- **Control signals within the processor**
- **Control signals to the control bus**

There are three types of control signals:
- **ALU function**
- **Data paths**: internal flow of data between registers
- **System bus**: control signals to control lines of system bus (memory READ)

## Hardwired Implementation
With this implementation, the control unit is essentially a state machine. Input logic signals are transformed into a set of output logic signals. The key inputs are the IR, clock, flags, and control bus signals. Usually a decoder is used to transform each opcode into a single signal that is high or low, to simplify the combinational logic. Then using the opcode, the instruction cycle state, and the other input sugnals, a boolean logic expression is derived for each control signal.

## Microprogrammed Control
This is mainly used for CISC systems. Their instructions are broken down into RISC-like instructions which are stored in a control memory. This control memory holds the microinstructions. A sequence of microinstructions is called a microprogram, or firmware. Each microintstruction simply generates a set of control signals. 

A microinstruction contains a control word, consisting of the output control signals, a jump/no jump condition, and the address of the next microinstruction if the jump condition is met.  
