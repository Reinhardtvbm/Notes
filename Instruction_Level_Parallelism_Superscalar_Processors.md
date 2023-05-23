# 1. Overview of Superscalar Processors
A machine that is designed to improve the perfomance of the execution of scalar instructions. The reason for designing such a system is that the majority of instructions operate on scalar quantities. 

Speedup in these superscalar computer systems is achieved by adding the ability to execute instructions independantly and concurrently in different pipelines. We can exploit this ability by changing the order of operations to best suit this new architecture without changing the intended operation of the program. 

Parallelism is achieved by adding more pipelines, allowing the processor to execute streams of instructions in parallel. Multiple instructions are performed at the same time where possible.

# 2. Constraints
## 2.1 Instruction-level parallelism
**The degree to which instructions can be executed in parallel** 

Compiler optimisations and hardware techniques attempt to maximise instruction-level parallelism

## 2.2 Dependencies
- True data dependency
- Procedural dependancy
- Resource dependancy
- Output dependancy
- Antidependancy

# 3. Design Issues
## 3.1 Instruction-Level Parallelism and Machine Parallelism
Instruction-level refers to the ability of instructions to be run at the same time, whereas machine parallelism refers to the computer system's ability to facilitate parallelising of instructions.

## 3.2 Instruction Issue Policy
The machine must be able to identify instruction-level parallelism, and perform fetching, decoding, and execution in parallel. **Instruction issue** is the process of initiating execution of an instruction using the processor's functional units. 
**Instruction issue policy** is the protocol, or method used to do this.

### 3.2.1 In-Order issue and in-order completion
Instructions are issued in the order as with sequential execution, and also written in that order

### 3.2.1 In-Order issue and out-of-order completion


### 3.2.1 Out-of-order issue and out-of-order completion
