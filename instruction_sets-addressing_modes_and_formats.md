# Instruction Sets: Addressing Modes and Formats
## Addressing Modes
There are seven common addressing modes:
- Immediate
- Direct
- Indirect
- Register
- Register indirect
- Displacement
- Stack

### Immediate Addressing
The simplest form of addressing, in which the operand value is present in the instruction. 

This is used for constants or to set initial values. The size of an immediate value is limited by the width of the instruction's address field. 

### Direct Addressing
The instruction contains the *effective address* (actual memory address) of the operand. This was common in earlier computers, but not so much anymore. Only one memory reference is required. The problem with direct addressing is that the address space that can be used with it is limited by the width of the address field in the intruction.

### Indirect Addressing
The solution to direct addressing's address range issue is indirect addressing. Here the instruction contains the address which refers to the effective address. In other words, the address given by the instruction points to the memory location which holds the address of the operand.

We can now address a space of size `s^N` for a word size of `N`. However, now two memory references are required. The number of *effective addresses* that can be addressed is still limited by the width of the address field. However, this is typically not an issue. 

### Register Addressing
The address field refers to a register in the CPU. The operand value is contained within the register.

### Register Indirect Addressing
The operand is at the address specified in the register specified by the address field in the instruction.

### Displacement Addressing
`EA = A + (R)`
