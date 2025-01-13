Instructions are the smallest unit in program. An instruction set contains all possible instructions that a computer can use. 

Instruction Set Architecture (ISA) is a design that defines the **interface between computer hardware and software**. It specifies the **instructions** supported by a processor, their **formats**, **behavior**, and **characteristics**. The ISA includes the **set of instructions** that a processor can execute, along with the corresponding **opcode formats**, **byte order**, **register set structure**, and **instruction functionalities**. It serves as a crucial interface between software and hardware, determining how software interacts with hardware and significantly impacting the performance and functionality of a computer system.
The design of an ISA typically encompasses:
1. **Instruction Set**: Defines the set of instructions supported by the processor, including arithmetic, load/store, control instructions, etc.
2. **Instruction Format**: Specifies the byte representation of instructions, including opcode, operand bits, addressing modes, etc.
3. **Register Set**: Includes the available registers in the processor, along with their number, size, and functions.
4. **Byte Order**: Specifies the storage order of multi-byte data types in memory, such as big-endian or little-endian.


##### Types of ISAs
The most beginner-friendly ISAs are MIPs and LC-3. 
There are also other types, such as:
- HP precision architecture: an instruction for `A * B + C`
- x86 ISA: multimedia extension (MME), later SSE and AVX
- VSA ISA: opcode to save all information of one program prior of switching to another program (multithread)
- PDP-x
- VAX
- IBM 360
- CDC 6600
- SIMD ISAs: CRAY-1, Connection Machine
- VLIW ISAs: Multiflow, Cydrome, IA-64 (EPIC) 
- PowerPC, POWER
- RISC ISAs: Alpha, MIPS, SPARC, ARM, RISC-V


##### **Classes**:
Nearly all ISAs today are classified as general-purpose register architectures, where the operands are either registers or memory locations. The popular versions are register-memory ISAs and load-store ISAs. 
- Register-memory ISA: In a register-memory ISA, instructions can directly operate on data stored in both registers and memory.
- Load-store ISA: In a load-store ISA, instructions only operate on data stored in registers, and memory access is restricted to specific load and store instructions.

##### **Memory addressing:**
Memory addressing refers to the process of locating and accessing data in computer memory. In a computer system, each storage location has a unique address used to identify that location. When a processor needs to access or manipulate data in memory, it must specify the address of the desired data.
Virtually all desktop and server computers, including the 80x86, ARM, and MIPS, use **byte addressing** to access memory operands. An access to an object of size s bytes at byte address A is aligned if `A mod s = 0`. For some architectures, it require an alignment of memory addressing. 

##### **Addressing mode**:
In addition to specifying registers and constant operands, addressing modes specify the address of a memory object. 

1. **Immediate Addressing Mode**: 
   The operand is part of the instruction. 
   `MOV A, #5` (Move the value 5 to register A)
   Notice that this operation there does not involved any load from memory, which is the major difference to relative addressing mode. From LC-3 the operation is `DR <- PC^ + sign-extend(PC_offset)`
2. **Direct Addressing Mode**: 
   The address of the operand is given directly in the instruction. 
   `MOV A, 30H` (Move the data from memory address 30H to register A)
3. **Indirect Addressing Mode**: 
   The address of the operand is stored in a register. 
   `MOV A, @R0` (Move the data from the address pointed to by register R0 to register A)
   In LC-3 the operation gives `Memory[Memory[PC^ + sign-extend(PC_offset)]]`, where `PC^` is incremented PC. Consider indirect addressing mode as a pointer chasing, where first address (inner bracket) specifies the address of the pointer, and second address (outer bracket) specifies the data value address stored within the pointer. 
   The advantage of this mode, is that it improves the limitation in relative jump where now the address of the operand can be anywhere in the memory.  
4. **Register Addressing Mode**: 
   The operand is stored in a register. 
   `MOV A, B` (Move the data from register B to register A)
5. **Relative Addressing Mode**: 
   The address of the operand is based on a displacement relative to the program counter (PC). 
   `JMP 20H` (Move the program counter to PC+20H)
   In LC-3, there is a **PC-relative addressing**
   The operation gives `Memory[PC^ + sign-extend(PC_offset)]` where `PC^` denotes the incremented PC. Note that this relative jump is limited to the amount of `PC_offset` register or bits can encode (for instance, given `PC_offset9` the memory address is limited to relative range of +22 to -256). Notice that the programmer must know where the data is relatively to current address, which is a bit outdated operation. 
6. **Base Addressing Mode (Base + offset mode)**: 
   The address of the operand is a combination of a base register and an offset. 
   `MOV A, 0(R1)` (Move the data from the address pointed to by register R1 plus offset 0 to register A)
   In LC-3 the operation is `Memory[BaseR + sign-extend(offset)`, now it adds the flexibility in addressing, since an address is stored in a base register it can also access anywhere in the memory. 
7. **Indexed Addressing Mode**: 
   The address of the operand is a combination of an index register and an offset. 
   `MOV A, 20H(X)` (Move the data from the address given by the contents of register X plus offset 20H to register A)
8. **Displacement Addressing**:
   In displacement addressing mode, the effective address of the operand is calculated by adding a constant displacement to a base register. The displacement is specified as part of the instruction, while the base register holds the starting address of the memory location.
   `MOV AX, [BX+10]` moves the contents of the memory location at address (BX + 10) into register AX.
   There are some variation of displacement addressing supported by 80x86 family. That is, 
   - **No Register (Absolute)**: The effective address is calculated using a constant displacement without involving any registers.
    `MOV AX, [1234h]`
   - **Two Registers (Based Indexed with Displacement)**: The effective address is calculated by adding a constant displacement to the sum of contents of two registers. It is, Effective Address = Base Register + Index Register + Displacement. 
    `MOV AX, [BX + SI + 10]`
   - **Two Registers with Scaling (Based with Scaled Index and Displacement)**: Similar to the previous mode, but one of the registers is multiplied by the size of the operand in bytes before adding the displacement. It is, Effective Address = Base Register + (Index Register * Scaling Factor) + Displacement
    `MOV AX, [BX + SI*2 + 10]`



There are five addressing modes in LC-3:
- **Immediate or literal (constant)**: the operand is in some bits of the instruction
- **Register**L the operand is in one of `R0` to `R7` registers
- **Memory addressing modes**:
	- **PC-relative**
	- **Indirect**
	- **Base + offset**
MIPS has pseudo-direct addressing (for `j` and `jal`), but does not have indirect addressing. 


The support of different addressing mode can be seen in the hardware, where different sources of address were multiplexed.  
![[address modes paths.png]]
##### **Types and sizes of operands**:
Like most ISAs, 80x86, ARM, and MIPS support operand sizes of 8-bit (ASCII character), 16-bit (Unicode character or half word), 32-bit (integer or word), 64-bit (double word or long inte- ger), and IEEE 754 floating point in 32-bit (single precision) and 64-bit (double precision). The 80x86 also supports 80-bit floating point (extended double precision).



##### Data types 
An ISA supports one or several data types. 
LC-3 only supports 2's complement integers. 
MIPs supports 2's complement integer, unsigned integer and floating point. 
Tradeoffs are involved in consideration of the design. Examples such as 
- Hardware complexity vs Software complexity
- Latency of operation on supported vs non-supported data types

When a microarchitecture supports more data types, usually it has the following:
Advantage:
- Enables better mapping from high-level programming constructs to hardware, (i.e., small number instructions and code size)
Disadvantage:
- Microarchitecture must be designed with more work


##### Complexity 
Instruction complexity is also related to complexity of data types. 
The term **Semantic gap** refers to how close the instruction sets & data types is to high-level  programming language. 
![[semantic gap.png]]
**Complex instructions** refer to instruction that do lots of work, such as:
- Insert a double linked list
- Compute FFT
- String copy
- Matrix operations
- Multi-dimensional array
Advantages: 
- Denser coding: advantages of denser codings are, smaller code size, better memory utilization, saves off-chip bandwidth, better cache hit rate (better packing for instructions)
- Simpler compiler: no need to optimise small instructions as much 
Disadvantages: 
- Larger chunks of works: compiler has less optimisations to do (limited to fine-grained optimisation)
- More complex hardware: translation from a high level to control signal and optimisation need to be done by hardware


**Simple instructions**, on the other hand, refer to instruction that do little work (but the primitive using which complex instruction can be built):
- ADD
- XOR

A translator can be used to create a better semantic outlook, that the build a bridge between high-level program with low-level hardware. However the translated signals will not be programmer-visible. 
![[translator in semantic gap.png]]

Some example of translator is Rosetta 2 binary translator and NVIDIA Denver. 


##### Operations:
The general categories of operations are **data transfer**, **arithmetic & logical**, **control**, and **floating point**.

![[instructions.png]]

##### **Control flow instructions**:
Virtually all ISAs, including these three, support **conditional branches**, **unconditional jumps**, **procedure calls**, and **returns**. All three use **PC-relative addressing**, where the branch address is specified by an address field that is added to the PC. 
**Status register flags** are updated after the condition codes:
- If written value is **negative**, N is set, Z and P are cleared
- If written value is **zero**, Z is set, N and P are cleared
- If written value is **positive**, P is set, N and Z are cleared. 

![[NZP flag in hardware.png]]

##### Types of instructions
There are some main types of instructions. Here is one way to classify them:
1. **Operation instructions**: Execute operations in the ALU
2. **Data movement instructions**: Read from or write to memory
3. **Control flow instructions**: Change the sequence of execution

##### **Encoding the ISA**:
Encoding an Instruction Set Architecture (ISA) involves defining the format and structure of machine instructions used by a processor or computer architecture. The encoding scheme determines how instructions are represented in binary form, including opcode, operand fields, addressing modes, and other relevant information.
There are two basic choices on encoding: fixed length and variable length. Variable-length instructions can take less space than fixed-length instructions. Note that choices mentioned above will affect how the instructions are encoded into a binary representation. For example, the number of registers and the number of addressing modes both have a significant impact on the size of instructions, as the register field and addressing mode field can appear many times in a single instruction.
In general there are several key points to consider when encoding an ISA:
- instruction format
- opcode assignment
- operand coding 
- addressing mode
- encoding rules
- instruction encoding table
- documentation
- validation and testing

Here are some example of instruction encoding:
![[instruction set encoding.png]]
`rs, rt: source registers`
`rd: destination register`
`shamt: shift amount (in shift operation)`
`funct: operation code in R-type instruction`

##### Tradeoffs
Many tradeoffs and considerations are in ISA design. Include the following:
- Execution model - sequencing model and processing style
- Instruction length
- Instruction format
- Instruction types
- Instruction complexity & simplicity
- Data types
- Number of registers
- Addressing mode types
- Memory organisation (address space, addressability, endianness, etc)
- Memory access restrictions and permissions
- Support for multiple instructions execute in parallel? 


##### Instruction cycle
**Instruction cycle** is a step or sequence that an instruction goes through to be executed. There are main six phases:
1. **Fetch**: Obtain the instruction from memory and loads it into the **Instruction Register** (common to every instruction type)
   Complete description:
   1. Load the `MAR` with the contents of PC, and simultaneously increment the PC
   2. Interrogate memory. This results in instruction being placed in `MDR` by memory 
   3. Load the IR with the content of `MDR`
2. **Decode**: 
3. **Evaluate address**
4. **Fetch operands**
5. **Execute** 
6. **Store result**
Not all the instructions have all phases (`ldi` does not require execute, `add` does not require evaluate address.) It is called instruction **cycles** because when a store result is completed, it fetches the next command. 

**Fetch**:
Step 1: In fetch operation, `PC` (containing the memory address information) will write to the memory address register (`MAR`) (through `gatePC` tri-state buffer, while other tri-state buffer should be 0 or a short circuit otherwise). A control signal of `LD.MAR` is set to write enable, the `MAR` is then updated.  The `PC` is then incremented by 1, loaded back by `PCMUX`. Note that this is in **hardware parallel**, meaning two actions can execute simultaneously. 

Step 2: The memory is then fetched given by the address. This is loaded to `MDR` (both 16 bits). `LD.MDR` is set to high again for write enable. 

Step 3: The `IR` is latched with the content of `MDR`, note that other control gates then `MDR` should be turned off. 

![[截圖 2024-05-24 下午1.36.40.png]]

**Decode**
The decoder will identify the instruction to be executed, and also generates some control signals (represented as white arrows) for processing the instruction. 
![[截圖 2024-05-24 下午1.41.32.png]]

The **finite state machine** of the decoder is shown as below. A list of executable instructions is branched, using decoded signals (referred as **decode state**) to identify which branch. 
![[finite state machine.png]]



**Evaluate address**
In this stage, a adder is used for adding 
**Sign-extend** refers to replicating the top bit of the data. 
After that, the address is generated by adding `SR1` value to offset indicated by `IR`, that is
`DR <- Memory [BaseR + sign-extend(offset6)]`
Finally the address is gated onto bus, for next cycle. 
![[截圖 2024-05-24 下午1.46.50.png]]


**Fetch operands**
Similar to `Fetch`, the signal is loaded into `MAR` and the content is fetched into `MDR`, then transfer back onto the bus. 
Note that the only difference is the memory source - from `Fetch`, `MAR` is from `PC` while in `Fetch operands`, the memory is from `Evaluate address` step. 
![[截圖 2024-06-03 上午11.39.13.png]]

**Execute**
In execute phase, some operation will be conducted. The following example is an `ADD` operation, where two target register were added together. 
In this example, the register address of `SR1`, `SR2` need to be specified by the control signal (on the side), and `ALU` must conduct `ADD` by giving correct command by `ALUK` control signal (from finite state machine). 
`SR1`, `SR2` are encoded in `IR`, the control signal is connected with `IR`. 

If the instruction is `ADI` (add immediate), `SR2MUX` will switch the channel from a source of a register to an immediate. The control signal is received from finite state machine (instruction decoder). 
![[截圖 2024-06-03 上午11.25.17.png]]



**Store result**:
The calculated result (16 bits) from `ALU` (from `Execute` stage) is transferred into register file (through tri-state buffer). The control signals in register files are `DR` (destination register, specified by `IR`) and `LD.REG` (register write enable signal). 
![[截圖 2024-05-24 下午1.20.20.png]]

If the result source isn't from `ALU`, it could be from `MDR` from the memory (such as `LDR`). The advantage of using a bus is, whenever a transfer of signal is needed, use tri-state buffer control and bus it can control the source the destination. 
![[截圖 2024-06-03 下午12.01.56.png]]

**Supplementary notes**: 
1. **Tri-state buffer**
   A tri-state buffer (or tri-state logic buffer) is an electronic device or a circuit that has three states: **high**, **low**, and **high-impedance** (often referred to as "Z"). This functionality is particularly useful in systems where multiple circuits need to **share a common data bus** without **interfering with each other**.
   The control of tri-state buffer is as followed:
   - **Enable High**: Output is the same as the input (high / low). 
   - **Enable Low**: Output is in a high-impedance state (effectively disconnected).
   
   The application of a tri-state buffer is used in **data buses**, **memory management** and **direction and state** in **microcontroller I/O pins**. 

