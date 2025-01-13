Concurrency
Data dependency

##### Issues

Pipeline stalls:
1. Resource contention
   Solution: 
   1) Eliminate the cause of contention. Duplicate the resource or increase its throughput (e.g., )
   2) Detect the resource contention and stall one of the contending stages.  
2. Dependences (between instructions)
   Dependences dictate ordering requirements between instructions. Namely there are two sources of dependences:
   - Data
     - **Flow dependence** (true data dependence - read after write)![[flow dependence.png]]
     - **Anti dependence** (write after read)![[anti dependence.png]]
     - **Output dependence** (write after write)![[output dependences.png]]
     Flow dependences always need to obeyed because they constitute true dependence on a value
   - Control
   
3. Long-latency (multi-cycle) operations

During the stall, the pipeline will do:
1. Stop all up-stream stages
2. Drain all down-stream stages


**Interlocking**: Detection of dependence between instructions in a pipelined processor to guarantee correct execution. 
Hardware interlocking vs software interlocking

**Scoreboarding**: Each register in register file has a valid bit associated with it. An instruction that is writing to the register resets the valid bit. An instruction in decode stage checks if all its source and destination registers are valid. Otherwise the pipeline is stalled. 

Advantage: simple, 1 bit per register (valid bit)
Disadvantage: need to stall for all types of dependences, not only flow dependences. 


**Combinational dependence check logic**: Special logic checks if any instruction in later states is supposed to write to any source register 

Six fundamental ways of handling flow dependences:
1. Detect and wait until value is available in register file
2. Detect and forward/bypass data to dependent instruction
3. Detect and eliminate the dependence at the software level 
    - No need for the hardware to detect dependence
4. Detect and move it out of the way for independent instructions
5. Predict the needed value(s), execute “speculatively”, and verify
6. Do something else (fine-grained multithreading)
	- No need to detect


Data forwarding / bypassing

Stall: make the dependent instruction wait, until the dependency is resolved:
1. 


##### Processor design
Implementation of stalling:
1. Disable PC and IF/IF latching
2. Insert invalid of the dependent process. 

However the nop (stalling) is costly in the waiting time. 

**Data forwarding / bypassing** is to forward the result value to the dependent instruction as soon as the value is available. 
It brings a pipeline closer to dataflow execution principles. 
![[data forwarding (with dist = 1).png]]
The cost is 
The implementation is that, the data is forward to execution stage from either memory or writeback stage. This is occurred when
1. If that stage will write to a destination register and the destination register matches the source register
2. If both the Memory & Writeback stages contain matching destination registers, Memory stage has priority to forward its data, because it contains the *more recently executed* instruction

The logic of 
```python
if ((rsE != 0) AND (rsE == WriteRegM) AND RegWriteM) then ForwardAE = 0b10 # forward from Memory stage
else if ((rsE != 0) AND (rsE == WriteRegW) AND RegWriteW) then ForwardAE = 0b01 # forward from Writeback stage
else ForwardAE = 0b00 # no forwarding
```


However, data forwarding is not always possible. Consider the following example, `lw` instruction does not finish reading the data until the memory stage, so it cannot be forwarded in the execution stage. 
![[截圖 2024-07-08 下午2.26.09.png]]
Some usual cases when forwarding is not possible:
- due to pipeline design and instruction latencies
- The `lw` instruction does not finish reading data until the end of Memory stage (its result cannot be forwarded to the Execute stage of the next instruction unless we want a long critical path) -> breaks critical path design principle
- 

Control flow
**Branch prediction**

branch misprediction penalty: (number of instructions flushed when branch is incorrectly predicted)

![[flush on misprediction.png]]

Early branch resolution
Advantages: 
- Reduce branch mispredictions => reduce CPI
Disadvantages:
- Potential increase in clock cycle time 
- Additional hardware cose

Usually, an important program consists of:
- 25% loads
- 10% stores
- 11% branches
- 2% jumps
- 52% R-type

Cycle time is determined by the slowest stage
![[截圖 2024-07-08 下午3.00.43.png]]



Hardware vs Software responsibility:
- Software based interlocking
- Hardware based interlocking
- Who inserts / manages the pipeline bubbles?
- Who finds the independent instructions to fill "empty" pipeline slots?
- What are the advantages / disadvantages of each?

Software: static scheduling
Hardware: dynamic scheduling


##### Fine-grained multithreading
The idea is fetch from a different thread every cycle such that no two instructions from a thread are in the pipeline concurrently. 

Requirements:
- Hardware has multiple thread contexts (PC + registers per thread)
- Threads are completely independent
- No instruction is fetched from the same thread until the prior branch / instruction 1from the thread completes. 

Advantages:
- No logic needed
Disadvantages:

Not all instructions take the same amount of time in the execute stage. 
![[violation of sequential execution due to various operation lengths.png]]

As can be seen here, multi-cycle execution pipelining does not follow the sequential semantics. If the DIV incurs an exception (e.g., DIV by zero), then it should identify an error and exit.  
![[Out of order execution.png]]

#### Exception and interrupts handling

**Exceptions**: Internal problems in execution of the program 
- Divide by zero
- Overflow
- Undefined opcode
- General protection / access protection
- Page fault
Handling occasion: when detected (and known to be non-speculative, such as branch predictions might be false)
Priority: process

**Interrupts**: External events that need to be handles by the processor
- I/O device needing service (e.g., keyboard, video inputs)
- System timer expiration
- Power failure
- Machine check
Handling occasion: when convenient (except for very high priority interrupts such as power failure or machine check error)
Priority: depends 

In finite state machine, the exceptions state flow is as follows:
![[FSM with exception handling routine.png]]



- Reorder buffer
  The idea is to complete instruction out-of-order, but reorder them before making results visible to architectural state. 
  ![[ROB for in-order retirement.png]]
  When instruction is decoded, it reserves the next sequential entry in a special buffer called the reorder buffer (ROB).
  When instruction completes, it writes result into ROB entry. 
  When instruction oldest in ROB and it has completed without exceptions, its result moved to reg. file or memory. 

   Reorder buffer is implemented as a circular queue in hardware, it is a hardware structure that keeps information about all instructions that are decoded but not yet retired / committed.  
  ![[ROB entries.png]]![[out of order execution, following sequential update order.png]]
  Another question raised is, what if a later instruction needs a value in ROB?
  Previous option is to stall the operation (which stall the pipeline). A better option comes with ROB is to directly read the value from ROB. 
  This complicates the machine into: ![[ROB with in-order retirement, with content addressable memor.png]]
  
  So now the operands can be fetched from register file, ROB or bypass path from data forwarding. 
  Register file and ROB has a different memory fetching mode. Register file uses **random access memory**, which by a given Reg. ID it will be used as the address of the entry. Meanwhile ROB uses **content addressable memory**, 

  Simplification of ROB:
  The idea is to use indirection: 
  1. access register file first (check if the register is valid)
   - If register not valid, register file stores the ID of the reorder buffer entry that contains (or will contain) the value of the register
   - Mapping of the register to a ROB entry: register file maps the register to a reorder buffer entry if there is an in-flight instruction writing to the register (adding a tag, i.e., a pointer to ROB entry), this remove the content addressability which significantly reduce the hardware and computation expenses.  ![[Intel ROB.png]]
  2. access ROB next, 
![[out of order completion with in order retirement.png]]
- History buffer
- Future register file
- Checkpointing

The idea of renaming register, is based on the fact that **output and anti dependences are not true dependences**. Hence the register IDs are renamed as ROB entries. 