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


##### **Classes**:
Nearly all ISAs today are classified as general-purpose register architectures, where the operands are either registers or memory locations. The popular versions are register-memory ISAs and load-store ISAs. 
- Register-memory ISA: In a register-memory ISA, instructions can directly operate on data stored in both registers and memory.
- Load-store ISA: In a load-store ISA, instructions only operate on data stored in registers, and memory access is restricted to specific load and store instructions.

##### **Memory addressing:**
Memory addressing refers to the process of locating and accessing data in computer memory. In a computer system, each storage location has a unique address used to identify that location. When a processor needs to access or manipulate data in memory, it must specify the address of the desired data.
Virtually all desktop and server computers, including the 80x86, ARM, and MIPS, use **byte addressing** to access memory operands. An access to an object of size s bytes at byte address A is aligned if `A mod s = 0`. For some architectures, it require an alignment of memory addressing. 

##### **Addressing mode**:
In addition to specifying registers and constant operands, addressing modes specify the address of a memory object. 

1. **Register Addressing**:
   In register addressing mode, the operand or data is directly specified by a register. The instruction contains the register number or identifier as part of the opcode.
   `MOV AX, BX` moves the contents of register BX into register AX.
2. **Immediate Addressing**:
   In immediate addressing mode, the operand is a constant value or immediate data specified directly in the instruction. The value is encoded as part of the instruction itself rather than being stored in memory.
   `MOV AX, 10` moves the immediate value 10 into register AX.
3. **Displacement Addressing**:
   In displacement addressing mode, the effective address of the operand is calculated by adding a constant displacement to a base register. The displacement is specified as part of the instruction, while the base register holds the starting address of the memory location.
   `MOV AX, [BX+10]` moves the contents of the memory location at address (BX + 10) into register AX.
   There are some variation of displacement addressing supported by 80x86 family. That is, 
   - **No Register (Absolute)**: The effective address is calculated using a constant displacement without involving any registers.
    `MOV AX, [1234h]`
   - **Two Registers (Based Indexed with Displacement)**: The effective address is calculated by adding a constant displacement to the sum of contents of two registers. It is, Effective Address = Base Register + Index Register + Displacement. 
    `MOV AX, [BX + SI + 10]`
   - **Two Registers with Scaling (Based with Scaled Index and Displacement)**: Similar to the previous mode, but one of the registers is multiplied by the size of the operand in bytes before adding the displacement. It is, Effective Address = Base Register + (Index Register * Scaling Factor) + Displacement
    `MOV AX, [BX + SI*2 + 10]`


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



##### **Operations**:
The general categories of operations are **data transfer**, **arithmetic & logical**, **control**, and **floating point**.

![[instructions.png]]

##### **Control flow instructions**:
Virtually all ISAs, including these three, support **conditional branches**, **unconditional jumps**, **procedure calls**, and **returns**. All three use **PC-relative addressing**, where the branch address is specified by an address field that is added to the PC. 

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

