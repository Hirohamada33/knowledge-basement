 ![[von Neumann model.png]]

**Five fundamental components**:
- **Memory (program and data)**
- **Processing unit**
- **Input**
- **Output**
- **Control unit (control the order in which instructions are carried out)**

There are two main properties that make von Neumann model successful, they are: 
1. **Stored program**
 - Instructions stored in a linear memory array
 - Memory is unified between instructions and data. The interpretation of a stored value depends on the control signals
2. **Sequential instruction processing**
 - One instruction processed (fetched, decoded & executed, completed & continued with next instruction) at a time
 - Program counter (instruction pointer) identifies the current instruction
 - Program counter is advanced sequentially except for control transfer instructions


An illustration of theoretical von Neumann model:
![[von Neumann model illustration.png]]
In this diagram:
- **Control signal** is represented as an white pointer. 
- **Tri-state buffer** is represented as a triangle. 
##### Memory Unit
Bits are logically grouped into bytes (8 bits) and words (e.g., 8, 16, 32 bits, depending on the architecture). It can represent program or data. 


###### Address space
Total number of uniquely identifiable locations in memory. 
Address space refers to the range of memory addresses that a computing system or a process can access. It encompasses the entire range of memory locations that are available for storing data and instructions. The size of the address space is determined by the architecture of the processor and the addressing scheme used by the system.

In a computer system, the address space is typically divided into several regions, including:
1. **Code Segment**: This region contains the machine code instructions of a program. It is where the executable code resides, and the processor fetches instructions to execute.
2. **Data Segment**: The data segment stores variables, constants, and other data used by the program during its execution. It includes both initialized and uninitialized data, such as global variables, static variables, and dynamically allocated memory.
3. **Stack**: The stack is used for storing local variables, function parameters, return addresses, and other information related to function calls and execution contexts. It grows and shrinks dynamically as functions are called and return.
4. **Heap**: The heap is an area of memory used for dynamic memory allocation, where memory is allocated and deallocated at runtime using functions like malloc() and free() in languages like C and C++.
###### Addressability
Addressability refers to the accessibility or identifiability of storage units (such as memory or external devices) within a computer system. It describes how the computer system identifies and accesses storage locations for reading or writing data.

Addressability typically depends on the hardware design and software support of the computer system. In terms of memory, addressability is determined by the number of address lines, which dictates the number of storage units the system can access. For example, a 32-bit system has 32 address lines and can therefore access $2^{32}$ (approximately 4 GB) storage units. Similarly, a 64-bit system has more address lines and can access more storage units.

**Word addressable** means the system can see a word (size dependent) as a unit of memory address. 
![[word addressing.png]]

**Byte addressable** means the system can see a byte as a unit of memory address. Usually a byte is commonly used in ISAs, hence byte addressability is also important for a system. 
![[byte addressing.png]]

In terms of instruction, if it is a word-addressable memory, the processor increments the PC by 1. If it is a byte-addressable memory, the processor increments the PC by the instruction length in bytes. 
###### **Endian**
There are big endian and small endian mode. 
- Big endian mode:
- Small endian mode: 
- 
![[endian modes.png]]

Although the convention does not really matter, it is important that on both side of the communication the endian mode should be agreed on. 



###### **Accessing memory**
To access the memory, the two ways are reading (loading) data from a memory location, and writing (storing) data to a memory location. 
There are two registers usually used to access memory: memory address register (`MAR`)
and memory data register (`MDR`)
**Read**:
1. Load the `MAR` with the address we wish to read from 
2. Data in the corresponding location gets placed in `MDR`

**Write**:
1. Load the `MAR` with the address and the `MDR` with the data we wish to write
2. Activate `Write Enable` signal → value in `MDR` is written to address specified by `MAR`

##### Processing unit
The processing unit performs the actual computation. It can contain many functional units. 

**Arithmetic Logical Unit (ALU)** is one of the example of a processing unit. It deals with arithmetic and logical operation. 
![[ALU and operations.png]]
Usually it is represented as the symbol above. An example with a list of ALU operations is provided. 


**Fast Temporary Storage (registers)**
It is almost always the case that a computer provides a small amount of storage very close to ALU. 
An example is $((A+B)*C)/D$, the intermediate result of $A+B$ can be stored in temporary storage, since the interaction with memory unit for an intermediate can be a slow process (store and retrieve). This temporary storage is usually a set of registers (also referred as **Register File**)
The registers in the processing unit ensure fast access to values to be processed in the ALU.  
Typically one register contains one word (same as word length). 
(Programmer visible)

General purpose register
special purpose register
![[register conventions.png]]
Above is an example for register file conventions for MIPS. 


##### Input & Output
##### Control unit
A control unit conducts the step-by-step process of executing (every instruction in) a program (in sequence). Essentially a computer can be seen as a finite state machine, and control unit take cares of the transition of a state to the next. 

An **instruction register (IR)** contains the actual instruction bits to keep track of which instruction is being processed. 
A **program counter (PC)** / **instruction pointer (IP)** contains a memory address of the next instruction to be executed.  

![[memory structure and PC.png]]



**Programmer visibility** refers to that the programmer can directly manipulate the storage or state. This marks the difference between microarchitecture and architecture, or simply hardware and software. 


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