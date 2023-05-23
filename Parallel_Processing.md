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

f simple time shared bus provides *simplicity*, *flexibility*, and *reliability*, but cannot always facilitate the performance one is aiming for when using an SMP system. This can be remedied by gifting a *personal cache* to each processor, which should reduce the frequency of bus accesses. 

### 2.2 Multiprocessor Operating System Design Considerations
Most OSs have to consider these, since the vast majority of personal computers have more than one processor. 
- Simultaneous concurrent processes
- Scheduling
- Synchronisation
- Memory management
- Reliability and fault tolerance

## 3 Cache Coherence and the MESI Protocol


## 4 Multithreading and Chip Multiprocessors 


