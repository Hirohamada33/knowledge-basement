**Processing in memory** means placing the compute capability in the memory or in the storage, while **processing near memory** means placing the elements or processing units near the memory or storage. 

The inspiration came from Hybrid memory cube - a 3D-stacked memory technology with multiple DRAM layers with a logic layer where the memory controller and some processing elements reside (containing some computational / processing units). The idea is with the layer underneath a memory bank (processing near memory) it can have much larger memory bandwidth (8TB/s). 
![[截圖 2025-01-06 下午3.33.19.png]]
##### Key system trends:
1. Data access is a major bottleneck
   - Applications are increasingly data hungry
2. Energy consumption is a key limiter
3. Data movement energy dominates compute
   - Especially true for off-chip to on-chip movement

Some of the application that has dense operations such as multilayer perceptor, neural network exploits data parallelism efficiently with optimisation technique such as tiling or bring large chunks of data in caches hierarchy, which is very compute efficiently by CPU / GPU. 
Some other includes the sparse memory access is not efficient for CPU / GPU hence a good candidate for PIM. 

The execution flow looks like below. 
![[截圖 2025-01-06 下午3.07.32.png]]
The overall operation is controlled by the host processor. The host processor uploads the computation to the NMP units. 
While the NMP is executing, the CPU is checking for a specific status register that is memory mapped which indicates when the computation is done. 



![[截圖 2025-01-06 下午3.12.04.png]]
Above is another architecture that is under development. For each bank it has a processing units, for each processing unit internally it has a multiply and accumulate units, and some units for activation function. 
![[截圖 2025-01-06 下午3.13.53.png]]
The supplementary SRAM buffer (global buffer) can be used for, temporary storage for data movement or, for instance, the global data from host processor such as the image for neural network inference. 


![[截圖 2025-01-06 下午3.17.29.png]]
This is a 3D-stacked memory system developed by Alibaba. Each has a DRAM die and a logic die, bonded by a special technique called hybrid bonding, which allows a lot of connection between the DRAM and logic die (large bandwidth). 
In DRAM die there are decoder, control logic, buffer, iOS #review_later and memory storage, while logic die has the necessary logic to access the memory controller. 
Alibaba additionally add some engines for specific operations. Here are **neural engines** and **match engines**. 
![[截圖 2025-01-06 下午3.21.45.png]]
GEMM unit (located in neural engine) is kind of a systolic array (run for small neural network). 
Notice that the matching and ranking stage are typically done on CPU because of their less parallelism and more irregular accesses nature. 
The proposal is to add the engines, using processing near memory to replace the execution on CPU. 

#### Approaches
There are two main approaches 
- Processing using memory
- Processing near memory
![[截圖 2025-01-06 下午3.28.39.png]]


There are some features / characteristics for processing in memory units
**Nature (of computation):**
- **Using**: Use operational properties of memory structures 
- **Near**: Add logic close to memory structures

**Technology:**
- Flash, DRAM, SRAM, RRAM, MRAM, FeRAM, PCM, 3D, 

**Location**:
Sensor, Cold Storage, Hard Disk, SSD, Main Memory, Cache, Register File, Memory Controller, Interconnect, 

Now a tuple of the three determines “PIM type". One can combine multiple “PIM types” in a system. 

Below are the approaches:
![[截圖 2025-01-06 下午3.35.28.png]]

The natural question then asked is, 
1. What are the performance and energy benefits of using 3D-stacked memory as a coarse-grained accelerator?
   - By changing the entire system
   - By performing simple function offloading

2. What is the minimal processing-in-memory support we can provide?
   - With minimal changes to system and programming

The motivation comes from the demands of large in-memory graph processing. 
Secondly, for application scaling, by increase the number of cores / threads, there will be a saturation of performance happening, which is limited by the total bandwidth available to a core. 
![[截圖 2025-01-06 下午3.42.19.png]]

The key bottlenecks in graph processing are: (1) large processing (2) iterative process. Consider the example where, two friends might be sitting next to each other (memory locality) while their friends might be different side of the world. When processing the graph, the vertices locations may be a random access of memory because of the locality is not guaranteed. 
The efficiency of (1) time and energy consumption of data movement and (2) cache hits are small. 
![[截圖 2025-01-06 下午3.50.45.png]]

Potential solution is the graph processing system. In each core there is an in-order core, which also has prefetchers that can access to not only local DRAMs. The message queue is used for communicating computation instruction from one core to another. **Remote function calls** are the remote execution of instruction. 
![[截圖 2025-01-07 下午3.21.35.png]]

As mentioned previously the memory access might be irregular or non-local. This code is using the remote function call to update operation by **sending a request from a core to another core**. The upside of the code is that the operation can be done asynchronously, while there's no dependencies, a core can send an instruction to another core then continue its own operation, hence called **non-blocking remote function call**. A barrier can be added in case a delay is needed (dependencies, etc.) 
![[截圖 2025-01-07 下午3.31.09.png]]

Below is the operation flow. 
1. Send function address and argument to the remote core
2. Store the incoming message in the message queue (MQ)
3. Flush the message queue when it is full or a synchronous barrier is reached
![[截圖 2025-01-07 下午3.39.55.png]]

Another method is to prefetch the data from the remote core to the local core. 
![[截圖 2025-01-07 下午3.51.24.png]]

![[截圖 2025-01-08 下午1.32.18.png]]![[截圖 2025-01-08 下午1.31.03.png]]
This solution then has a much more performance improvement compared to the other in-order / out-of-order architecture, due to the **increased memory bandwidth**.  


#### Question:
1. Is physical distance a bottleneck of data movement? 
2. What is an accelerator?

