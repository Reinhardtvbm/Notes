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
