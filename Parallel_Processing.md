# Parallel Processing
## 1 Multiple Processor Organisation
Organisation of multiple processor systems come in four flavours
- SISD (Single Instruction, Single Data stream)
  - One processor executes a single instruction stream to operate on data in a single memory, i.e. normal single core CPU
- MISM (Multiple Instruction, Single Data stream)
  - Many instructions of can be performed on one piece of data (not very useful)
- SIMD (Single Instruction, Multiple Data stream)
  - A single instruction can be performed simultaneously on data in more than one location (GPUs)
- MIMD (Multiple Instruction, Multiple Data stream)
  - A set of processors can simultaneously execute many instructions on many datasets
  - SMPs, Clusters, NUMA

## 2 Symmetric Multiprocessors (SMP)
Symmetric meaning many processors that are similar or the same. 

In an SMP computer system, processor share the same memory and I/O, all processors share the same function, and the system is controlled by an operating system.

The potential advantage over the uniprocessor system are:
- Performance: more processors is better
- Availability: the failure of one processor does not spell doom for the entire system
- Incremental growth: performance can be enhanced by adding more processors
- Scaling: manufacturers/vendors can easily scale designs to provide many price/performance options

### 2.1 Organisation
In general, the processors, I/O, and memory are all connected by some interconnection network. Most commonly, this is a time shared bus. It functions very similarly to the bus in a uniprocessor system, consisting of control, address, and data lines. Howecer, direct memory access (DMA), requires the following:
- **Addressing**: of the source/destination of data on the bus
- **Arbitration**: Any I/O module can temporarily function as "primary"
- **Time-sharing**: obviously, only one module can control the bus at any one time

f simple time shared bus provides *simplicity*, *flexibility*, and *reliability*, but cannot always facilitate the performance one is aiming for when using an SMP system. This can be remedied by gifting a *personal cache* to each processor, which should reduce the frequency of bus accesses. Enter *cache coherence*.

### 2.2 Multiprocessor Operating System Design Considerations
Most OSs have to consider these, since the vast majority of personal computers have more than one processor. 
- Simultaneous concurrent processes
- Scheduling
- Synchronisation
- Memory management
- Reliability and fault tolerance

## 3 Cache Coherence and the MESI Protocol
When we organise a computer system such that each processor has its own cache, multiple copies of the same data can exist in each of their caches simultaneously. Furthermore, if each processor is allowed to update its copy of the data freely and independantly, the memory system as a whole becomes inconsistent. There are two common cache write policies: 
- **Write back**: write operations are only made to cache, and a piece of memory is updated when it is removed from cache
- **Write through**: Write operations are done to cache and main memory, ensuring data in main memory is valid

Using *write back* in a multiprocessing environment results in inconsistency. So, how did the smart computer engineers solve thisproblem?
### 3.1 Software Solutions
We can simply rely on compilers and operating systems to deal with the problem. However, software approaches need to be conservative in dealing with this problem, so we don't get the most out of the computer this way. *Compiler-based coherence mechanisms* analyse the code and mark those items that may become unsafe for caching. The OS or hardware can then ensure that these items are not cached. 

### 3.2 Hardware Solutions
We refer to thees as *cache coherence protocols*. They provide dynamic recognition at run time. This allows for a more effective use of cache. This is also hidden from the programmer/compiler, allowing them to use their brains less. They can be generally divided into two categories: **directory protocols**, and **snoopy protocols**.

#### 3.2.1 Directory Protocols
These collect and maintain information about where copies of lines reside. Typically in a centralised controller that is part of the main memory controller, and a directory that is stored in main memory. It contains state information about the contents of the various local caches. When an individual cache controller makes a request, the centralised controller checks and issues the appropriate commands for data transfer between memory and caches

### 3.3 The MESI Protocol
Each line has two status bits per tag, so that is can be in one of four states:
- **Modified**: the line is different from main memory and is only available in this cache
- **Exclusive**: the line is the same as in main memory and is only present in this this cache
- **Shared**: the line is the same as in main memory and exists in more than one cache
- **Invalid**: the line does not contain valid data

## 4 Multithreading and Chip Multiprocessors 
Multithreading is achieved by splitting the instruction stream into several smaller streams, known as threads, which can be executed in parallel.

### 4.1 Implicit and Explicit Multithreading
- **Process**: an instance of a program running on a computer; it has two key characteristics:
  - **Resource ownership**: has its own virtual address space for its process image. a process may be allocated control over resources in the computer system
  - **Scheduling/execution**: The execution of the process follows an execution path (trace) through one or more programs. This execution may be interleaved with other processes. Thus a process has an execution state.
- **Process switch**: An operation that switches the processor from one process to another, by saving the process' information to create room for another.
- **Thread**: A dispatchable unit of work within a process. Includes a processor context, and its own data area for the stack. It executes sequentially and is interruptible so that the processor can turn to another thread.
- **Thread switch**: The act of switching processor control from one thread to another within the same process. Typically less costly than a process switch.

