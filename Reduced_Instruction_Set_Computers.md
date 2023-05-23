# Chapter 17: Reduced Instruction Set Computers

## 17.1  Instuction Set Characteristics
The drop on cost of hardware means that the relative cost of software has increased. Along with the chronic shortage of programmers, and the ubiquitous unreliability of software, the absolute cost of software has increased.
The development of ever more complex high-level languages. These HLLs aim to solve this problem by:
    * allowing algorithms to be expressed more consisely
    * allow the compiler to handle details unimportant to the expression of the algorithm
    * support the natural use of structured programming paradigms
This has, however, led to the *semantic gap*, which is the disconnect between instructions implemented in **HLLs** and those implemented in the hardware. This causes the complexity of compilers to increase, excessive machine program size, and execution inneficiency.

To remedy these issues, HLL statements were implemented in hardware leading to ever more complex hardware architecture and organisation.

However, other research was also being done on simplifying the architectures that support HLLs, rather than increasing their complexity.

This is where RISC comes in. Its aim is to optimise the most time-consuming features in HLL programs, rather than to imitate their functionality. These features include loops, assignments, operand references, and procedure calls/returns.

Following is a discussion of the three main elements that emerge when designing for these optimisations. These elements characterise RISC architectures.

### 17.2  The Use of a Large Register File
HLLs result in the need to *quick access to operands*, specifically local scalars, as well as *assignment statements*. This suggests a heavy reliance on *register storage*.
Register storage is the fastest storage mechanism available to the computer system. It is physically small, and uses short addresses. One then stores the most frequently accessed operands in the registers and minimises register-memory operations.

This can be achieved by relying on the compiler to maximise register usage. This requires sophisticated program-analysis algorithms. Anothere approach is to simply increase the number of registers.

#### Register Windows
The other consideration is the passing of variables between outer and inner scopes during procedure calls and returns. To facilitate this for one function call, we use two register windows, which consist of _Parameter registers_, _Local registers_, and _Temporary registers_. The temporary registers of the window used for the scope that calls the function and the Parameter registers of for the scope of the function being called overlap. When the function returns, changes made to the values in these registers are reflected, since these "overlapping" registers are the same.

#### Global Variables
The window scheme allows for the efficient storage of local variable, however global variables requires some other mothod. 
One could just have global variables be referenced in memory, but an alternative is to have a small set of registers specifically for global variables, and have the linker decide which globals get assigned to which registers.

#### Large Register File vs Cache
Using a large register file causes inefficient use of space within the CPU, since not all register windows will be fully occupied at any one time. However, using cache means that data is stored in memory blocks, meaning that some or most of the block will be unnecessary data. Furthermore, a cache will dynamically discover gloabal variables. 
When using a register file, the movement of data between registers/memory is determined by procedure nesting depth. This depth fluctuates within a narrow range, therefore memory is not often used. However, if only cache were used, then we run the risk of having other data/instructions compete for space in the cache, due to the fact that set-associative caches with small set sizes are most often used. 
There is however, one way in which a register file far outperforms a cache. That is the addressing overhead. A relatively simple decoder can be used to reference a register in the CPU, but when using cache, we need to find the set using a longer address and also compare tag bits to determine if the reference is a hit or not. Furthermore, the addressing mode also affect the amount of time it takes to reference a piece of memory in the cache.

## 17.3  Compiler-Based Register Optimisation
The objective of the compiler is to keep the operands for as many computaions as possible in registers. 
The general procedure for this is to assign each symbol used in the HLL to a virtual register. Then each set of virtual registers that does not have overlapping usage can be assigned to a unique physical register. If there are too many quantities for the number of registers, then these excess quantities are stored in memory. 

The essence of the optimisation task is to decide which quantities are to be assigned to registers at any given point in the program. For this we use **GraphColouring**. SEE PAST PAPERS :)

## 17.4  Reduced Instruction Set Architecture
### Why CISC?
Simplify compilers and improve performance. Underlying both of these was the trend towards ever more abstract HLLs. 

### Characteristics of Reduced Instruction Set Architectures
    * One instruction per cycle
    * Register-to-register operations
    * Simple addressing modes
    * Simple instructionn formats

#### One Instruction per machine cycle
A *machine cycle* is defined as the time it takes to fetch two operands from registers, perform an ALU operation, and store the result in a register. Thus a RISC instruction should be no more complicated than, and execute as fast as microinstructions in CISC machines. This means there is little or no need for microcode. Such machine instructions should execute faster than those on other machines since there is no need to access a microprogram control store.

#### Register-to-register Operations
Most operations are register-to-register, with only simple LOAD and STORE operations used to access memory. This simplifies the instruction set, and therefore, the Control Unit. RISC ISAs may include only two ADD instructions (normala and with carry), but VAX has 25 different ADD instructions. Another benefit of this, is that optimisation of register use is encouraged, so that frequently accessed operands remain in high-speed storage. 


## 17.5  RISC Pipelining
Most RISC instructions have only two stages: Instruction Fetch, and Execute, sometimes memory access is also required as a third stage.

### Optimisation of Pipelining
#### Delayed Branch
#### Delayed Load
#### Loop Unrolling

