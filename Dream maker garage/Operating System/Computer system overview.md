An OS exploits the hardware resources of one or more processors to provide a set of services to system users. The OS also manages secondary memory, allocation of resources, and I/O devices on behalf of its users. 
It provides a basis for application programs and acts as an intermediary between computer user and hardware. 
![[截圖 2024-11-24 下午8.38.55.png]]

**Types of OS**:
- Batch OS
- Time-sharing OS
- Distributed OS
- Network OS
- Real Time OS
- Multi- programming / processing / tasking OS
#### Basic elements
- **Processor**: controls the operation of the computer and performs its data processing functions.
- **Main memory**: stores data and programs. This memory is typically volatile. Usually referred as real memory or primary memory
- **I/O modules**: move data between the computer and its external environment memory devices, communications equipment, and terminals . 
- **System bus**: provides for communication among processors, main memory, and I/O modules. 

![[截圖 2024-11-24 下午6.43.39.png]]

#### Evolution
1. **Microprocessors**: Though originally much slower than multichip processors, microprocessors have continually evolved to the point that they are now much faster for most computations due to the physics involved in moving information around in sub-nanosecond timeframes.
2. **Multiprocessors**: In addition, there are now **multiprocessors**; each chip (called a socket) contains mul- tiple processors (called cores), each with multiple levels of large memory caches, and multiple logical processors sharing the execution units of each core.
3. **GPU**: Graphical Processing Units (GPUs) provide efficient computation on arrays of data using Single- Instruction Multiple Data (SIMD) techniques pioneered in supercomputers. GPUs are no longer used just for rendering advanced graphics, but they are also used for general numerical processing, such as physics simulations for games or computations on large spreadsheets. Simultaneously, the CPUs themselves are gaining the capabil- ity of operating on arrays of data–with increasingly powerful vector units integrated into the processor architecture of the x86 and AMD64 families.
4. **DSP**: Digital Signal Processors (DSPs) are also present, for dealing with stream- ing signals–such as audio or video. DSPs used to be embedded in I/O devices, like modems, but they are now becoming first-class computational devices, especially in handhelds. 

Other specialised computational devices (fixed function units) co-exist with the CPU to support other standard computations, such as **encoding/decoding speech and video (codecs)**, or providing support for **encryption and security**.

To satisfy the requirements of handheld devices, the classic microprocessor is giving way to the **System on a Chip (SoC)**, where not just the CPUs and caches are on the same chip, but also many of the other components of the system, such as DSPs, GPUs, I/O devices (such as radios and codecs), and main memory.

#### Instruction execution cycle
A program to be executed by a processor consists of a set of instructions stored in memory. There are essentially two steps involved: fetching stage and executing stage. The processing required for a single instruction is called an instruction cycle.
![[截圖 2024-11-24 下午6.52.07.png]]

A halt status occurred only if the processor is turned off, some sort of unrecoverable error occurs, or a program instruction that halts the processor is encountered. 

The processor interprets the instruction and performs the required action. In general, these actions fall into four categories:
1. **Processor-memory**: Data may be transferred from processor to memory or from memory to processor.
2. **Processor-I/O**: Data may be transferred to or from a peripheral device by transferring between the processor and an I/O module.
3. **Data processing**: The processor may perform some arithmetic or logic operation on data.
4. **Control**: An instruction may specify that the sequence of execution be altered. 

![[截圖 2024-11-24 下午6.58.46.png]]
Both instructions and data are 16 bits long, and memory is organised as a sequence of 16-bit words. The instruction format provides 4 bits for the opcode, allowing as many as $2^4$ = 16 different opcodes (represented by a single hexadecimal1 digit). The opcode defines the operation the processor is to perform. With the remaining 12 bits of the instruction format, up to $2^{12}$ = 4096 (4K) words of memory (denoted by three hexadecimal digits) can be directly addressed.

![[截圖 2024-11-24 下午6.58.27.png]]
The program fragment shown adds the contents of the memory word at address 940 to the contents of the memory word at address 941 and stores the result in the latter location.

#### Interrupt
Virtually all computers provide a mechanism by which other modules (I/O, mem- ory) may interrupt the normal sequencing of the processor.

| Class                | Description                                                                                                                                                                                                                                             |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Program**          | Generated by some condition that occurs as a result of an instruction execution, such as **arithmetic overflow**, **division by zero**, attempt to execute an **illegal machine instruction**, and **reference outside a user’s allowed memory space**. |
| **Timer**            | Generated by a **timer** within the processor. This allows the operating system to perform certain functions on a **regular basis**.                                                                                                                    |
| **I/O**              | Generated by an I/O controller, to signal **normal completion of an operation** or to signal a **variety of error conditions**.                                                                                                                         |
| **Hardware failure** | Generated by a failure, such as **power failure** or **memory parity error**.                                                                                                                                                                           |

Interrupts are provided primarily as a way to improve processor utilisation. For example, most I/O devices are much slower than the processor. This could mean the processor do not need to idle when performing I/O devices-related operations. 

The diagram presents the effect of utilising the interrupt to prevent the long idling time. 
![[截圖 2024-11-24 下午7.36.50.png]]

With interrupts, the processor can be engaged in executing other instructions while an I/O operation is in progress. The I/O operation is conducted **concurrently** with the execution of instructions in the user program.
When the external device becomes ready to be serviced, the I/O module for that external device sends an **interrupt request** signal to the processor.


![[截圖 2024-12-15 下午3.13.01.png]]
To accommodate interrupts, an interrupt stage  is added to the instruction cycle. In the interrupt stage, the processor checks to see if any interrupts have occurred, indicated by the presence of an interrupt signal. 

Below is the timing diagram of both with or without the interrupt in an operation. 
![[截圖 2024-12-15 下午3.14.22.png]]

A typical I/O operation will take much more time than executing a sequence of user instructions (e.g., printer). Figure c represents the idea, that while I/O runs concurrently, the main code reaches the second call of I/O operation while first concurrent operation is still processing. Below is the timing diagram.  
![[截圖 2024-12-15 下午3.20.20.png]]
There is still a gain in efficiency because part of the time during which the I/O operation is underway overlaps with the execution of user instructions.

The interrupt processing has the following steps:
![[截圖 2024-12-15 下午3.23.57.png]]
Here are some clarification:
1. In step 3, the processor **tests for a pending interrupt request, determines that there is one**, and sends an acknowledgment signal to the device that issued the interrupt. The acknowledgment allows the device to remove its interrupt signal.
2. In step 4, PSW stands for **program status word**, where it contains status information about the currently running process, including **memory usage information**, **condition codes**, and other status information, such as an **interrupt enable/disable bit** and a **kernel/user mode bit**. 
3. In interrupt handling steps, depending on the computer architecture and OS design, **there may be a single program**, one for each type of interrupt, **or one for each device and each type of interrupt**. If there is more than one interrupt-handling routine, the processor must determine which one to invoke. This information may have been **included in the original interrupt signal**, or the processor **may have to issue a request to the device** that issued the interrupt to get a response that contains the needed information.
4. In addition to step 4, storing the PSW, additional relevant information must also be taken into consideration (e.g., contents of the processor registers).  In this case, a user program is interrupted after the instruction at location `N`.  The contents of all of the registers plus the address of the next instruction `N+1`, a total of `M` words, are pushed onto the control stack. The stack pointer is updated to point to the new top of stack, and the program counter is updated to point to the beginning of the interrupt service routine.
The storage of current state is important for later resumption as interrupt is rather a unpredictable occurrence than a regular routine call. Below is a demonstration storing current state before / after interrupt.
![[截圖 2024-12-15 下午3.42.35.png]]

###### Multiple interrupts
There could be multiple interrupts occurring. A common example is in a printer a communication interrupt is issued once received a batch of data meanwhile it could be in I/O operation (printing) which issues I/O interrupt. 
Several approaches are used:
1. **Sequential interrupt processing** simply disables the further interrupt once in ISR, and allow the upcoming interrupts remaining pending. They resumes once an interrupt has been dealt with. However, the drawback is it does not take into account relative priority or time-critical needs. (For instance, in communication interrupts data batch could be lost if delay is too long from over-buffering).  
2. **Nested interrupt processing** define priorities for interrupts and to allow an interrupt of higher priority to cause a lower-priority interrupt handler to be interrupted. 
![[截圖 2024-12-15 下午3.50.59.png]]

#### Memory hierarchy
The dilemma of the following relationship holds:
1. Faster access time, greater cost per bit 
2. Greater capacity, smaller cost per bit 
3. Greater capacity, slower access speed

Typically for the design consideration, as one goes down the hierarchy, 
1. Decreasing cost per bit 
2. Increasing capacity 
3. Increasing access time 
4. Decreasing frequency of access to the memory by the processor

A hit ratio $H$ is defined as the fraction of all memory accesses that are found in the faster memory. Suppose $H = 0.95$ in a two-level memory system, with level 1 access time to be $0.1\mu\text{s}$ and level 2 access time to be $1\mu\text{s}$, the average access time can be calculated by
$$(0.95) (0.1\mu\text{s})+ (0.05) (1\mu\text{s}) = 0.15\mu\text{s}$$
