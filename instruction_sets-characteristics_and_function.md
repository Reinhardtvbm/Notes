# Instruction Sets: Characteristics and Function
## Machine Instruction Characteristics
### Elements of a Machine Instruction
Each instruction must the information required by the processor for execution. 
- **operation code**: the operations to be performed (op code)
- **source operand reference**: operands that are inputs for the operation
- **result operand reference**: operands for placing the result of the operation
- **next instruction reference**: where the processor must fetch the next instrution after execution of this one is complete (for explicit references like in a GOTO, or BRA)

Source and result operands can be in one of four areas
- **main or virtual memory**
- **processor register**: there are very rare exceptions when the processor has no registers available to the programmer
- **immediate**: the value is the operand itself
- **I/O device**: for memory mapped I/O, this can be regarded the same as a memory address

### Instruction Representation
Instructions are divided into fields, each containing the instructions contituent elements.

Usually we use mnemonics for opcodes, such as ADD, SUB, or LOAD.

### Instruction Types
If we consider the following high level language statement
    `X = X + Y`

If we assume X corresponds to memory location 10, and Y corresponds to memory location 20, then we can accomplish this with three machine instructions:
- load a register with contents of memory location 10 (LOAD r, 10)
- add the contents of memory location 20 to the register (ADD r, 20)
- store the new value in the register at memory location 10 (STA r, 10)

So we require *three* machine instructions for one *very simple* high-level statement. The computer designer must ensure that the instruction set is sufficient to express any statement in a high-level language. So we have the following instruction types to accomplish that:
- **data processing**: arithmetic / logic instructions
- **data storage**: movement of data between registers and memory
- **data movement**: I/O instructions
- **control**: test and branch instructions

*Arithmetic* instructions allow processing of *numeric* data. *Logic* instructions operation of the bits in the data, allowing the programmer to process any other type of data. *Test* instructions are used to test the value of data or check the status of an instruction. 

### Instruction Set Design
The design of an instruction is very complex becuase if affects so many aspects of the computer system. The instruction set defines many functions performed by the processor and thus impacts its design greatly. It is the programmer's means of controlling the processor, therefore the programmer must be considered when designing the instruction set.

Many of the fundamental issues surrounding instruction set design remain disputed. The most important of these design issues are:
- **operation repertoire**: how many / which operations and how complex should they be
- **data types**: the types of data that the operations support
- **instruction format**: length of the instruction (bits), size of fields, order of fields
- **registers**: number of registers that can be referenced by instructions
- **addressing**: mode(s) by which the address of an operand is specified

## Types of Operands
The data that machine instructions operation on can be put into the following general categories:
- addresses
- numbers
- characters
- logical data

Some machines go beyond this and support specialised data / data structures such as vectors or matrices.
