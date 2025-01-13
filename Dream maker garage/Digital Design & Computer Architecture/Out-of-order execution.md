The pipeline machine significantly increase the speed by introducing stages. However, if an operation take a significant amount of time, the whole pipeline will halt. 
Consider the case below, where division is the long operation. 
![[截圖 2024-12-28 下午6.40.49.png]]
In this case, the worst-case instruction latency determines all instructions’ latency



The basic idea of out-of-order execution is to move the **non-ready instructions out of the way of independent ones** (such that independent ones can dispatch)

Reservation stations are the rest areas for non-ready instructions. 

Monitor the source “values” of each instruction in the resting(waiting) area. When all source “values” of an instruction are available, “fire” (i.e., dispatch) the instruction. In that way, instructions dispatched in dataflow (not control-flow) order

**Benefit**:  
- Latency tolerance: Allows independent instructions to execute and complete in the presence of a long-latency operation

![[comparison of pipeline for in-order and out-of-order design.png]]



![[two-humped processor design.png]]

##### Reorder buffer 
(Reorder buffer operation: [[https://www.youtube.com/watch?v=nkWVrZqW584]] 1:36:44 start) 
The idea of reorder buffer is to complete instructions out-of-order, but reorder them before making results visible to architectural state. 
When instruction is decoded, it reserves the next-sequential entry in a special buffer called the Reorder Buffer (ROB). 
When instruction completes, it writes result into ROB entry. 
When instruction oldest in ROB and it has completed without exceptions, its result moved to reg. file or memory (and will now be retired). 

A hardware structure that keeps information about all instructions that are decoded but not yet retired/committed. 
![[截圖 2024-12-28 下午6.47.12.png]]

Consider the storage for sufficient representation of a state. 
Everything required to:
1. correctly reorder instructions back into the program order
2. update the architectural state with the instruction’s result(s), if instruction can retire without any issues
3. handle an exception/interrupt precisely, if an exception/interrupt needs to be handled before retiring the instruction
Need valid bits to keep track of readiness of the result(s) and find out if the instruction has completed execution. 

An entry contains the following information:
![[截圖 2024-12-28 下午6.47.45.png]]

The following is an example of the reorder buffer operation. 
![[截圖 2024-12-28 下午7.08.44.png]]