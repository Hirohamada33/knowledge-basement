The memory unit contains **data** and **program** that exchanges with CPU. 

| Address| Type   |
| :----: | :----: | 
| address 0 | program | 
| address 1 | program |
| ...|...|
|address 31 | data |
|address 32 | data |

When a CPU can directly control the space, it is usually referred as **address space**. 

#### Bus

A bus is referring to the data tunnel where computer send and receive signals. 
There are **address bus** and **data bus**. Address bus specifies the address location and send to a memory unit, whereas memory unit returns the data via data bus, which the data is stored within the specified address given by address bus. 

External buses are the tunnels between CPU and external units, and internal buses are the tunnels between CPU itself. 


##### Bus width and bits 

Bus width refers to the number of the signal lines in the transmission. For instance, a 64-bit CPU can process 64 bits (i.e., 64 signal lines) at the same time. The wider the bus width is, suggesting the better the CPU processing capacity is. 

The width of external data bus indicates **number of bits of data can be transmitted** at once. The width of internal data bus indicates **number of bits of data can be operated (calculated)** at once. 
Meanwhile, the width of address bus indicates **number of bits of address can be processed** at once. 

The size of the address can be determined by the width of the address bus. For instance, an address bus with the width of 32 bits indicates this address space has the size of 2^32 bits (which is about 4.3 GB).  

#### MUX

MUX simplifies the wiring within CPU, by switching between tunnels. 
