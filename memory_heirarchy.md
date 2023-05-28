There are two types of memory locality:
- Temporal locality: temporal, meaning memory. As in, memory units recently used are likely to be referenced again in the near future
- Spatical locality: A program's tendency to require memory at addresses close to one another

## Characteristics of Memory Systems
### Key Terms
- **word**: the "natural" unit of organisation of memory. 
- **addressable unit**: the number of bits that can be addressed. Some 32-bit systems are Byte addressable, for example.
- **unit of transfer**: the number of bits read out of or written into memory during a single transaction

### Access Methods
- *sequential access*: such as tape units
- *direct access*: blocks are addressable, but some work must be done to find the specific piece of memory within the block
- *random access*: each addressable unit is immediately accessible with its address
- *associative*: data is found by comparing desired bit locations within a word for all words in memory simultaneuously

### Memory Perfomance Concepts
- **access time / latency**: The time it takes to perform a read/write operation. For memory that is not rendomly accessible, this time may vary
- **Memory cycle time**: Primarily used for random-access memory's. Consists of the access time plus and additional time required before the next memory access can take place
- **transfer rate**: 1/(*cycle time*)


