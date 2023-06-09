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

### 4.2 Approaches to Explicit Multithreading
There are four principle approaches to multithreading:
#### Interleaved multithreading
Also known as **fine-grained multithreading**. The processor deals with two or more thread contexts at a time, swithing from one thread to another at each clock cycle. If a thread is blocked because of data dependancies or memory latencies, that thread is skipped and a ready thread is executed.

#### Blocked Multithreading
Also known as **coarse-grain multithreading**. The instructions of a thread are executed successively until an event occurs that may cause delay, such as a cache miss. This even induces a switch to another thread. This approach is effective on an in-order processor that would stall the pipeline for a delay event such as a cache miss. 

#### Simultaneous multithreading (SMT)
Instructions are simultaneously issued from multiple threads to the execution units of a superscalar processor. This combines the wide superscalar instruction issue capability with the use of multiple thread contexts. 

#### Chip multiprocessing
In this case, multiple cores are implemented on a single chip and each core handles seperate threads. The advantage of this approach is that the available logic area on a chip is used effectively without depending on ever-increasing complexity in pipeline design. This is referred to a **multicore**. 

We can then contrast different pipelining architectures with and without multithreading:
- **Single-threaded scalar**: simple pipeline found in traditional RISC/CISC machine with no multithreading
- **Interleaved multithreaded scalar**: The easiest approach to implement. By switching threads at each clock cycle, the pipline stages can be kept fully occupied. The hardware must be able to switch threads between instruction cycles.
- **Blocked multithreaded scalar**: A single thread is executed until a latency even occurs that would stop the pipeline, at which time a thread switch occurs.

Remember that although a mutlithreaded scalar processor offers better processor utilisation, it does so at the cost of single thread performance. 

- **Superscalar**: basic superscalar with no multithreading. During some cycles, not all available issue slots are occupied, which is referred to as *horizontal loss*. During other cycles, no instructions can be issued, which is called *vertical loss*
- **Interleaved multithreaded superscalar**: During each cycle, as many instructions as possible from one thread are issued. Thread switching occurs between cycles.
- **Blocked multithreaded superscalar**: Like blocked mutithreaded scalar, but with superscalar pipeline. Can also be thought of as interleaved multithreaded superscalar, but thread switching only occurs with a latency event on the current thread.
- **Very long instruction word (VLIW)**: Multiple instructions are placed in a single word. Typically constructed by the comiler. Instructions that can be executed in parallel are placed in the same word. If it is not possible to completely fill a word with instructions, no-ops are used.
- **Interleaved multithreading VLIW**: similar efficiency to interleaved mutithreading superscalar
- **Blocked multithreading LVIW**

Final two approached enable parallel, simultaneous execution of multiple threads:
- **Simultaneous multithreading**: Capable of issueing multiple instructions at the same time. So horizontal slots can typically be filled by simply placing other threads in them, providing a high level of efficiency.
- **Chip multiprocessor**: The processor has seperate independant cores. The diagram in the textbook shows a quad-core processor, which each have a two-issue superscalar architecure, and can therefore issue up to two instructions per cycle. 
