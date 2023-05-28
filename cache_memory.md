# Cache Memory
## Cache Memory Principles
Cache memory contains blocks of data. Entire blocks are retrieved from memory upon a cache miss. The *spatial and temporal locality* of memory references makes this efficient, since if data from a memory location is required, then the other data in that block will likely be used as well.
- **Block**: The minimum unit of transfer between cache and main memory
- **Line**: A portion of cache memory capable of holding *one* block
- **Line size**: the width of the line, *not* including control and tag bits
- **Tag**: A portion of a cache *line* that is used for addressing purposes

Below is a figure of cache read operation:

![](cache_read_operation.png)

### Transfers from Main Memory to Cache
The typical line size of a cache is 128 Bytes, and the main memory transfer size is typically 64 *bits*. This means that transferring an entire block into cache cannot be done in one clock cycle. The pivotal technique employed here is **critical word first**, where the word requested by the processor is delivered first so it can continue execution while the cache continues transferring the rest of the block into the cache line.

## Elements of Cache Design
### Physical vs Logical Caches
Almost all nonembedded and most embedded processors support virtual memory. A cache that uses virtual addresses is called a *logical cache*, and one that uses physical addresses is called a *physical cache*. In computers that use physical caches, the cache must get the physical address from the MMU (memory management unit) which translates virtual addresses to physical ones. A logical cache can use the virtual address already in the operand.

### Cache Size
It needs to be small enough that the cost per bit is similar to that of main memory, but large enough that the *average access time* is close to the speed that would be acheived with cache alone. Larger caches require more gates to access, resulting in a slightly slower access time, even with the same IC technology. Available chip and board area also limits cache size. 

### Logical Cache Organisation
#### Direct Mapping
Each block of main memory can only belong to one line in the cache. No comparison of tag bits needs to occur to obtain the requested data.

The problem with direct mapping, is that if two different blocks that map to the same line in cache are being referenced repeatedly, then those two blocks are constantly swapped resulting in a low hit ratio (this is called *thrashing*)

#### Fully Associative Mapping
A block in main memory can be mapped to any line in cache. The tag field of each line is huge, and simultaneous comparison of every line's tag bits is used to find the line containing the referenced word.

This overcomes the thrashing issue with direct mapping, since a block can be put into any cache line.

The disadvantage of fully associative mappping is the complex circuitry required to examine the tag bits of every line in parallel.

#### Set-Associative Cache
Set-associative mapping maps a set of blocks in main memory to a set of lines in cache. This solves thrashing and does not require as many tag bits, reducing the circuit complexity. 

A set-associative cache that has *k* lines in each set is called a *k-way* set-associative cache.

### Replacement Algorithms
Once the cache line that the requested block is mapped to is full, there needs to be some strategy for opening up a slot for it. With direct mapping, there is only one line per set, so we simply discard the block that is in the line to make space for the new one. But for associative schemes, a *replacement algorithm* is necessary.

The most effective is **least recently used**. Where the line used *the least recently* is trashed. Good thing engineers name things well!

### Write Policy
When a block is to be replaced by an incoming block, if the outgoing block has been altered, then main memory must update the data as well. 
We run into two problems: more than one device may have access to main memory, and if a word is altered in cache, the corresponding word in main memory becomes invalid; similarly if an I/O device alteres a word in main memory, then the word in cache becomes invalid. This problem is even worse in multicore systems that employ independant caches for each core. 

The simplest technique is **write through**. Every write operation simply made to both main memory and cache. This ensures that main memory is always valid. This creates substantial memory traffic, which creates a bottleneck. An alternative technique is **write back**, which only ever updates cache. A **dirty bit** or **use bit** is used as a control bit in the cache line, and memory is written back to cache when the block is replaced only if the dirty bit is set. This means that I/O modules can be allowed *only through the cache*. 

Another dimension to write policy is how write *misses* are handles
- **write allocate**: the block to be altered is fetched from main memory, and the processor writes to it there
- **no write allocate**: the word is altered in main memory, and is not loaded into a cache line

*write allocate* is usually paired with *write back*, and *no write allocate* is paired with *write through*.

### Line Size
A larger line, correlates with an increased hit ratio. However, larger blocks decrease the number of blocks that can fit into the cache, resulting in more block replacements. Additionally, block sized too large cause the cache to start fetching words less likely needed, decreasing the hit ratio, and, of course, there is the amount of time that it takes to retrieve a larger block. 

### Number of Caches
More recenlty, the use of multiple cache levels has become the norm. When on-chip cache became the norm, the question was, "what if we kept the slightly slower off chip cache as well?". Typically, the answer is yes. 
