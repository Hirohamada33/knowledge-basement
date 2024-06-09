In terms of human and machine communication, it can be simplified into the following process:
(Human) Compiler -> Assembler -> Machine code (Computer)

Instructions are encoded in program memory, referred as *opcodes* or *machine codes*. They fall into roughly five categories:

- **Arithmetic and logic**
  Arithmetic operations deal with the numerical calculations. Such as addition, subtraction, multiplication, division, exponential, logarithm.
  Logic operators deal with the logic calculations. Such as AND, OR, NOT, EXOR. They are used mainly for binary data. 
- **Change of flow**
  The commands in this category control the flow of the program and sometimes change of the process of the program. Such as branching, jump, call, return.  
- **Data transfer**
  The commands in this category deal with the transfer of data, such as loading, copying, moving, exporting the data from a location to another. The location could be register, data space, I/O port, peripherals etc. 
- **Bit and bit-test**
  Bit commands are used to manipulate the status (on/off) of particular bits in a location. Such as set bit, clear bit, toggle bit. 
  Bit-test commands, on the other hand, is use to monitor and test the status of particular bits in a location, depending on the condition to further determine the flow of the program. Such as compare. 
- **Control**
  Control commands are use to manage the entire procedures of the program, including initialisation, exit, interrupts and error handling. 

Q: What is PC counter?
Program counter (PC) is used for tracking next instruction that is about to be executed. PC counter increments with the execution of the program, pointing it to the location in memory where the next instruction is stored. Program counter is an essential element in CPU, to ensure the sequence of the program execution. 


Assembly language has **directive** and **label** in the syntax. 
Directive is the command that is only used in assembly code, but will not be loaded as a machine code into CPU. The directives are used for defining macro, initialisation, include external files, etc. It is usually distinguished from other assembly command by keywords and pseudo-opcodes. 
Example such as `.section, .init0`and `.set`
Label marks the location in the program, the name of the label could be anything. The usage of marking location is mainly for flow control, such as jump, branch and call. It can also be used for defining the variable and the location of the data. Usually a label ends with a colon. 
Example such as `Loop:`, `Init:`.

**Variables**, in the context of assembly, usually represented by **registers**. As mentioned, registers can be used for **temporary variable storage**. The general approach is to use the register for variable storage, if ran out, then using stack for additional storage. 

Dissembling


**Branch, jump and skip**
For branch instructions, they interact with the SREG flags. It does the following operation:
1. Checks if a specified flag is set / cleared
2. If true, jump to a relative location PC <- PC + k + 1
3. Else, PC <- PC + 1
Essentially, there's only two basic commands for branch - checking if SREG flags are set and checking if SREG flags are cleared (BRBC - branch if bit in SREG is cleared, BRBS - branch if bit in SREG is set). Other instructions are just user-friendly alternatives. 

For skip instructions, they compare registers for equity / clear or set. It does the following process:
1. Checks if two registers are equal, or checks a bit in register of I/O space is set / cleared
2. If true, skip the next (or two) instruction.  PC <- PC + 2 or 3
3. Else, PC <- PC + 1



Q: Why does the amount of cycles of branch, skip different for true and false case?

