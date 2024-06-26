*A computer is a digital electronic machine that can be programmed to carry out sequences of arithmetic or logical operations (computation) automatically. Modern computers can perform generic sets of operations known as programs.*

#### Definition
The term computer architecture actually refers to the following task, instruction set design, functional organization, logic design, and implementation. 

##### Instruction set design
Refer to [[Instruction Set Architecture]]

##### Architecture
**Von Neumann architecture**
- Single (common) bus for instruction and data

**Harvard architecture**
- Separate busses for instruction and data

**Performance**:
Generally, with multiple bus lines there are some advantages such as:
- Higher percentage of cache hits
- Wider memory bandwidth

#### History

Two significant changes in the computer marketplace made it easier than ever before to succeed commercially with a new architecture. 
1. The virtual elimination of assembly language programming reduced the need for object-code compatibility. 
2. The creation of standardized, vendor-independent operating systems, such as UNIX and its clone, Linux, lowered the cost and risk of bringing out a new architecture.
These changes made it possible to develop successfully a new set of architectures with simpler instructions, called **RISC (Reduced Instruction Set Computer)** architectures. 
The RISC-based machines focused the attention of designers on two critical performance techniques:
1. The exploitation of **instruction-level parallelism** (initially through pipelining and later through multiple instruction issue)
2. The use of **caches** (initially in simple forms and later using more sophisticated organizations and optimizations).

The introduction of RISC-based architecture has replaced VAX.

**Supplementary notes**:
- **Instruction-level parallelism**
  Instruction level parallelism (ILP) is a technique in computer processors where multiple instructions are executed simultaneously. It aims to improve the efficiency and performance of the processor by reducing the waiting time for instructions through simultaneous execution, thereby achieving higher instruction execution speeds.
  In addition to ILP, there are also DLP (data-level parallelism), TLP (thread-level parallelism), RLP (request-level parallelism) 
- **Caches**
  Caches are high-speed memory storage locations that store frequently accessed data or instructions to reduce access time and improve overall system performance. They are used to bridge the speed gap between the fast CPU and slower main memory (RAM). Caches work on the principle of locality, which means that programs tend to access the same memory locations repeatedly or nearby locations.
- **Pipeline**
  A pipeline in computing refers to a technique used to improve the performance of a system by breaking down a task into **smaller, independent stages**, each of which can be executed **concurrently**. The idea is to divide the processing of instructions or data into **several stages,** allowing different stages of the process to **overlap in time**. This overlapping of stages helps to maximize the utilization of hardware resources and increase overall throughput.
  In a typical pipeline, each stage performs a specific operation on the input data or instruction before passing it to the next stage. The output of one stage becomes the input for the next stage, and so on, until the task is completed. Each stage operates independently and in parallel with the other stages, which allows multiple tasks to be processed simultaneously.

##### Classification of higher-level compilers

###### Performance-oriented languages
Performance-oriented languages are those optimized for execution efficiency, typically characterized by:
1. **Close-to-the-metal control**: These languages often provide low-level control over hardware details, allowing developers to optimize for specific hardware architectures.
2. **Efficient memory management**: They offer more direct and efficient memory management techniques, such as manual memory allocation and deallocation, to avoid excessive memory overhead and fragmentation.
3. **Compilation and runtime optimization**: They come with robust compilers and runtime systems capable of optimizing code for improved performance.
4. **Concurrency and parallelism support**: These languages often provide rich support for concurrency and parallel processing, making it easier for developers to implement multithreading and distributed processing.

Some common examples of performance-oriented languages include:
1. **C/C++**: These statically typed, compiled languages offer efficient memory management and low-level control, making them widely used for system programming and performance-sensitive applications.
2. **Rust**: Designed for system programming, Rust emphasizes safety, concurrency, and performance. It boasts powerful compile-time and runtime optimizations, along with extensive support for concurrency.
3. **Go (Golang)**: With its static typing and compiled nature, Go features concise syntax and efficient concurrency support, making it popular for network services and large-scale system development.
4. **Julia**: Julia is a high-performance dynamic language with execution speeds close to native code. It's known for its rich scientific computing and numerical analysis capabilities.


###### Managed programming languages
Managed programming languages are those where memory management and other low-level system tasks are handled by a **runtime environment** or a **virtual machine**, rather than by the programmer directly. This approach provides several benefits, including **automatic memory management (garbage collection)**, **platform independence**, and **enhanced security**. Common characteristics of managed programming languages include:
1. **Automatic Memory Management**: In managed languages, the runtime environment automatically allocates and deallocates memory for objects, relieving the programmer from **manual memory management tasks** and reducing the risk of **memory leaks** and **memory corruption**.
2. **Platform Independence**: Managed languages typically run on top of a virtual machine or a runtime environment, which abstracts away hardware differences. This allows programs written in managed languages to be easily ported across different platforms without modification.
3. **Security**: Managed languages often include built-in security features, such as type safety and memory bounds checking, which help prevent common security vulnerabilities like **buffer overflows** and **null pointer dereferences**.
4. **Performance Trade-offs**: While managed languages offer convenience and safety, they may incur a performance overhead compared to lower-level languages like C or C++. This overhead is primarily due to the additional runtime and memory management overhead.

Examples of managed programming languages include:
1. **Java**: Java is one of the most widely used managed languages, known for its "write once, run anywhere" mantra. Java programs run on the Java Virtual Machine (JVM), which provides automatic memory management and platform independence.
2. **C#**: Developed by Microsoft, C# is a managed language designed for building applications on the .NET framework. C# programs run on the Common Language Runtime (CLR), which handles memory management and other system tasks.
3. **Python**: Python is an interpreted, dynamically typed language with automatic memory management. While not running on a virtual machine like Java or C#, Python relies on a runtime environment that manages memory and provides other high-level features.
4. **JavaScript**: JavaScript is a dynamic, interpreted language commonly used for web development. Modern JavaScript engines include garbage collectors and other memory management mechanisms to handle memory automatically.

###### Scripting language
A scripting language is a high-level programming language used for automating tasks and executing scripts. Scripts typically consist of a series of instructions and commands used to perform specific tasks such as file processing, system administration, web development, data processing, and more. Scripting languages are commonly used to interact with software applications and operating systems to achieve automation and custom functionality.

Scripting languages possess the following characteristics:
1. **Ease of Learning and Use**: Scripting languages often have simple, easy-to-understand syntax and structure, allowing beginners to quickly get started.
2. **Flexibility**: Scripting languages are flexible and can dynamically adjust and extend based on task requirements. They support various programming paradigms such as object-oriented, functional programming, etc.
3. **Cross-Platform**: Most scripting languages are cross-platform, meaning they can run on multiple operating systems and environments, providing good portability.
4. **Efficiency**: While scripting languages may execute slower than compiled languages in some cases, they can often achieve the same functionality with less code and are quick to develop and test.
5. **Wide Range of Applications**: Scripting languages are widely used in various domains including system administration, network management, web development, data processing, scientific computing, game development, and more.


**Supplementary notes**:
- **Garbage collection**
  The garbage collector is responsible for automatically identifying and reclaiming memory that is no longer in use by the program, thereby avoiding memory leaks and memory fragmentation issues.
- **Memory leaks**
  A memory leak refers to a situation where a program allocates memory during its execution but **fails to deallocate it afterwards**. This results in a **continual consumption of memory**, potentially leading to the **exhaustion** of available memory resources and adversely affecting the program's performance. Common causes for memory leaks are: failure to release dynamically allocated memory, circular references, global and static variables
- **Memory corruption**
  Memory corruption refers to the unintended modification of data stored in memory. Unlike memory leaks, which involve the improper management of memory allocation and deallocation, memory corruption occurs when the content of memory is altered during program execution, often by external factors or programming errors. Some common causes for memory corruption: buffer overflows, dangling pointers, use-after-free, heap corruption, pointer arithmetic errors. 
- **Runtime environment**
  The software and hardware environment required for a program to run during its execution. It includes elements such as **runtime libraries**, **virtual machines**, **memory management**, **input/output management**, and more. These elements collectively provide a platform for running a program.
- **Buffer overflows**
  A buffer overflow occurs when a program tries to write more data into a buffer (a fixed-size memory storage area) than it was allocated to hold. This can lead to data beyond the buffer's boundary overwriting adjacent memory locations, which can corrupt data, crash the program, or even allow an attacker to execute arbitrary code.
  Buffer overflows are a common **security vulnerability** and can be exploited by attackers to gain **unauthorized access** to a system or **execute malicious code**. They are often caused by programming errors, such as not properly validating input size or using insecure string manipulation functions. If a buffer overflows, the attacker will be able to directly write to the memories, causing system to crash down or behave maliciously. 
  To prevent buffer overflows, programmers should follow best practices such as using **safe string manipulation functions**, **validating input data**, and using language features or libraries that offer **protection** against buffer overflows, such as **bounds checking** or **memory-safe languages**. Additionally, regular security testing and code reviews can help identify and mitigate potential buffer overflow vulnerabilities in software.
- **Null pointer dereferences**
  A null pointer dereference is an error that occurs when a program attempts to use a pointer that points to a null (empty) memory address. This error often leads to program crashes or abnormal terminations because it tries to access memory locations that do not exist. 
  A null pointer dereferencing can lead to the following issues: program clashes, undefined behaviour, security vulnerability. 


###### Classes of Parallelism and Parallel Architectures
In theory, there are two types of parallelism:
1. **Data-Level Parallelism (DLP)** arises because there are many data items that can be operated on at the same time.
2. **Task-Level Parallelism (TLP)** arises because tasks of work are created that can operate independently and largely in parallel

Computer hardware in turn can exploit these two kinds of application parallelism in four major ways:

1. **Instruction-Level Parallelism** exploits data-level parallelism at modest levels with compiler help using ideas like pipelining and at medium levels using ideas like speculative execution.
2. **Vector Architectures** and **Graphic Processor Units (GPUs)** exploit **data-level parallelism** by applying a single instruction to a collection of data in parallel.
3. **Thread-Level Parallelism** exploits either data-level parallelism or task-level parallelism in a **tightly coupled hardware model** that allows for **interaction** among parallel threads.
4. **Request-Level Parallelism** exploits parallelism among largely decoupled tasks specified by the programmer or the operating system.


Computer architecture is shown as follow:

![[computer architecture diagram.png]]


Microprocessor consists of control unit ad arithmetic logic unit, and microcontroller consists of CPU, memory, input and output. 


There are different types of memory: <b>flash, EEPROM </b> and <b> SRAM. </b>

Q: What are the difference? 

Flash memory:
1. Non-volatile
2. Inexpensive
3. Slower than SRAM
4. Can only erase in chunks
5. Typically used to store program data

SRAM:
1. Volatile
2. Fast  
3. Read/write individual bytes  
4. Typically used to store variables

EEPROM:
1. Non-volatile
2. Expensive  
3. Can erase individual bytes




Here is the layout of the AVR1626 microcontroller. 

![[AVR1626 layout.png]]

The AVR core has the following components:
- RISC (Reduced Instruction Set Computer, 8-bit)
    
- Registers (32 pairs)
    
- Program counter (PC)
    
- Arithmetic logic unit (ALU)
    
- Status register (SREG)
    
- Stack pointer (SP)
	


CPU interacts with memory and peripherals. Code (or instructions) are stored in code space (flash), meanwhile,  memory and peripherals are located in data space. 

Q: What is a peripheral?
A peripheral refers to an external device, that provides additional functionality or input/output capability. For instance: joystick (input peripherals), projector (output peripherals), USB flash drive(storage peripherals), router (network peripherals).

Sometimes, peripheral register can be used to drive a external device. It is used to store, monitor and modify the status and information of the peripherals. Each register is assigned for a unique address in data space.

When a peripheral is accessed in such method (the external device data can be accessed via internal register), it is referred as "memory-mapped" (內存映射). When it is memory mapped, the peripheral register is mapped to the assigned internal data space. This will allow the processor to access the data via internal data space, same as the process of visiting RAM, and read/write to the device, instead of writing to I/O port.  In short term, the use of peripheral registers and memory mapping make the interactions and data transmission easier. 

Q: What is I/O space?
Usually there are two types of space in general computer architecture: memory address space and I/O address space. 
Memory address space stores program-related data, including program counter, program commands, variable etc. 
I/O address space, on the other hand, usually is used for communicating with external devices. Peripherals can be allocated into internal system by I/O space or peripheral registers. By using I/O space, it can ensure the commands and operations will not affect the peripherals when dealing with internal register. Hence, the commands used for operations in I/O space are usually different with the commands for registers, to avoid the conflicts. 




![[memory space.png]]

From the perspective of the CPU, the data space is single large array of locations that can be read from, or written to, using an address.


Below is a **levels of transformation** diagram. It describe the bridge from abstract problem to a circuitry / device level solver. 

![[截圖 2024-06-15 下午4.56.47.png]]

**Algorithm**
An algorithm is a step-by-step procedure that is guaranteed to terminate, such that each step is precisely stated and can be carried out by the computer. There are terms to describe each of these properties.
We use the term **definiteness** to describe the notion that each step is precisely stated. A recipe for excellent pancakes that instructs the preparer to “stir until lumpy” lacks definiteness, since the notion of lumpiness is not precise.
We use the term **effective computability** to describe the notion that each step can be carried out by a computer. A procedure that instructs the computer to “take the largest prime number” lacks effective computability, since there is no largest prime number.
We use the term **finiteness** to describe the notion that the procedure terminates.