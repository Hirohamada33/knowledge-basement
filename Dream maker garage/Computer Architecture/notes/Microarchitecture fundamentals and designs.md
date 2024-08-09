
Some discussions about the property of ISA vs microarchitecture
- ADD instruction opcode (ISA)
- Type of adder used in the ALU (bit-serial vs ripple-carry) (microarchitecture)
- Number of general purpose registers (ISA)
- Number of cycles to execute the MUL instruction (microarchitecture)
- Number of ports to the register file (microarchitecture)
- Whether or not the machine employs pipelined instruction execution (microarchitecture)
- Program counter (ISA)

The process instruction step:
- ISA specifies abstractly what AS' should be, given an instruction and AS
  - It defines an abstract finite state machine where state = programmer-visible state, next-state logic = instruction execution specification
  - From ISA point of view, there are no "intermediate states" between AS and AS' during instruction execution. There is one state transition per instruction
- Microarchitecture implements how AS is transformed to AS'
  - There are many choices in implementation. 
  - We can have programmer-invisible state to optimise the speed of instruction execution: **multiple** state transitions per instruction:
	- Choice 1: `AS -> AS'` (transform AS to AS' in a **single clock cycle**)
	- Choice 2: `AS -> AS + MS1 -> AS + MS2 -> AS + MS3 -> AS'` (take **multiple clock cycles** to transform AS to AS') 

Single-cycle vs Multi-cycle machines:
**Single-cycle machines**:
- Each instruction takes a single clock cycle, also referred as serialised processing (i.e., all six phases of the instruction processing cycle take a single machine clock cycle to complete)
- All state updates made at the end of an instruction’s execution
- Control signals are generated in the same clock cycle as the one during which data signals are operated on
- Big **disadvantage**: The slowest instruction determines cycle time -> long clock cycle time
**Multi-cycle machines**:
- Instruction processing broken into multiple cycles/stages (i.e., each phase of all six phases of instruction can take multiple clock cycles to complete)
- State updates can be made during an instruction’s execution
- Architectural state updates made at the end of an instruction’s execution
- Control signals needed in the next cycle can be generated in the current cycle
- Latency of control processing can be overlapped with latency of datapath operation (more parallelism) 
- **Advantage** over single-cycle: The slowest “stage” determines cycle time


###### Performance analysis
Execution time of single instruction is given by `{CPI} x {clock cycle time}`
Execution time of an entire program is given by `{no. of instructions} x {Average CPI} x {clock cycle time}` or sum over all instructions `{CPI} x {clock cycle time}`
**Single-cycle microarchitecture performance**:
- CPI = 1
- Clock cycle time = long  
**Multi-cycle microarchitecture performance**:
- CPI = different for each instruction
- Average CPI -> hopefully small
- Clock cycle time = short
- 2-3 degrees of optimisation

##### Instruction processing engine
An instruction processing engine consists of two components:
1. **Datapath**: Consists of hardware elements that deal with and transform data signals. This includes:
   - functional units that operate on data
   - hardware structures (e.g., wires, muxes, decoders, tri-state bufs) that enable the flow of data into the functional units and registers
   - storage units that store data (e.g., registers)
2. **Control logic**: Consists of hardware elements that determine control signals, i.e., signals that specify what the datapath elements should do to the data 

##### Single-cycle machine design from ground up
###### Elements
- **Program counter**: 32-bit register
- **Instruction memory**:Takes input 32-bit address A and reads the 32-bit data (i.e., instruction) from that address to the read data output RD
- **Register file**: The 32-element, 32-bit register file has 2 read ports and 1 write port
- **Data memory**: If the write enable, WE, is 1, it writes 32-bit data WD into memory location at 32-bit address A on the rising edge of the clock. If the write enable is 0, it reads 32-bit data from address A onto RD.
###### Assumptions

###### Arithmetic and Logic instructions
###### Data movement instructions
###### Control flow instructions

