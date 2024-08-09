The basic idea of out-of-order execution is to move the non-ready instructions out of the way of independent ones (such that independent ones can dispatch)

Reservation stations are the rest areas for non-ready instructions. 

Monitor the source “values” of each instruction in the resting(waiting) area. When all source “values” of an instruction are available, “fire” (i.e., dispatch) the instruction. In that way, instructions dispatched in dataflow (not control-flow) order

**Benefit**:  
- Latency tolerance: Allows independent instructions to execute and complete in the presence of a long-latency operation

![[comparison of pipeline for in-order and out-of-order design.png]]



![[two-humped processor design.png]]


