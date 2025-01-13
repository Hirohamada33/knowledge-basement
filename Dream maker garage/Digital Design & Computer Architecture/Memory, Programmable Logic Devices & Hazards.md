##### Terminology
- **Memory cell**: circuit that stores 1 bit of data
- **Memory word**: a group of memory bits 
- **Capacity**: e.g. 4096 20-bit word (equiv. 81920 bits)
- **Address**
- **Read / Write operation**: (fetch and store operation)
- **Access time**: Time taken for data to arrive after an operation request
- **Cycle time**: Minimum time between two operations 

##### Random access memory (RAM)
RAM is volatile (loses information if power switched off) 
- **Dynamic RAM (DRAM)** - General, main memory: Fast, Gbit capacity, but limited data retention time (must be ‘refreshed’)
- **Static RAM (SRAM)**: - Cache memory, for continuous access: Very fast, up to Mbit capacity, data refreshing not needed
![[截圖 2024-11-25 上午11.09.09.png]]

##### Read-only memory (ROM)
A ROM cell can store 1 bit of information. Data can be read but not changed (written). ROM is non-volatile (the data can be read after turning the power on again). 
Applications are:
- permanent storage of programs for microprocessors
- look-up tables of data
- implementing combinational logic

![[截圖 2024-11-25 上午11.15.23.png]]
The implementation of ROM is an activation of the whole row by the select signal $A[2:0]$, followed with a multiplexer with the select signal $A[5:3]$ to choose the correct cell.
Notice that half of the address inputs are used for selecting row and half are for selecting column. 
Row select energises all switch transistors in one row, and column select uses multiplexer to target one column. 
Outputs are high usually but can be pulled down if a cell is programmed to do so. 

![[截圖 2024-11-25 上午11.22.07.png]]
A voltage is stored to represent a 0 (or 1) as required. 
If the “row line” is addressed, the switch closes and the stored voltage appears on the “column line”. 
The switch is implemented with a (MOS) transistor.


###### Storage Mechanism
The storage mechanism for the 0 or 1 depends on the design of the ROM. 

![[截圖 2024-11-25 上午11.32.24.png]]
**Masked Programmed ROM** is programmed at the time of manufacture. The switch transistor is made to have a low threshold voltage to program a 0 and a high threshold to program a 1. 

![[截圖 2024-11-25 上午11.33.42.png]]
**Field Programmable ROM** is programmable using a PC system. A semiconductor fuse is blown when writing a logic level of 1 (irreversible process). 


##### Programmable logic device
$2^n$ x 1 bit ROM devices have n input and 1 output. They can be used to implement logic directly, such as the truth table below is implementation of $\text{OUT} = XY+Z$ 
![[截圖 2024-11-25 上午11.38.28.png]]
- Program the first 8 addresses according to the OUT column
- Connect X to A2, Y to A1 and Z to A0 of the ROM
- Foundation of programmable gate arrays
The main advantages of programmable logic device are:
- reduction in chip count  
- easy upgrade by just reprogramming

![[截圖 2024-11-25 上午11.37.14.png]]
PALs (programmable array logic) are programmed like PROMs using fuses. It is one-time programmable, and for unconnected `AND` inputs there will be pull-up resistor. 


The architecture of an example of a function implementation is presented. 
![[截圖 2024-11-25 上午11.40.35.png]]

##### Static hazard
Gates have finite propagation delay. A "static 0 hazard" refers to a glitch from 0 -> 1 -> 0 and "static 1 hazard" refers to a glitch from 1 -> 0 -> 1. 

For instance, an implementation of the function $f(A, B, C) = AB + A\overline{C}$ is given by
![[截圖 2024-11-25 上午11.45.12.png]]
The generation of $\overline{A}$ will have a propagation delay due to the inverter. 

In order to avoid static hazard,  
1. Use a Karnaugh map and look for groups of minterms which do not overlap, to identify the potential static hazard. 
2. Avoid the hazard by introducing additional groups so that no non-overlapping groups remain

In the following example, a group BC may have a potential static hazard in transition ($ABC$ and $\overline{A}BC$). In order to avoid the hazard, it is introduced the additional redundant term $BC$
![[截圖 2024-11-25 上午11.47.12.png]]

Hence the overall function implementation gives $\overline{A}C + AB + BC$. 
When there is a transition, the third term will remain 1 to ensure there are no transitional glitching. 

