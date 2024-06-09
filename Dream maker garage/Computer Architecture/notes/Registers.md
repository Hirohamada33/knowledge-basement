#### Status registers


![[status registers.png]]

There are 8 bits of status registers presented serving different purposes. 

###### Bit 7 – I: Global Interrupt Enable:
The **Global Interrupt Enable** bit must be set for the **interrupts** to be enabled. The individual interrupt enable control is then performed in separate control registers. If the Global Interrupt Enable Register is cleared, none of the interrupts are enabled independent of the individual interrupt enable settings. The **I-bit is cleared** by hardware after **an interrupt has occurred**, and is set by the `RETI` instruction to enable subsequent interrupts. The I-bit can also be set and cleared by the application with the `SEI` and `CLI` instructions, as described in the instruction set reference.
###### Bit 6 – T: Bit Copy Storage:
The Bit Copy instructions `BLD` (bit load) and `BST` (bit store) use the T-bit as source or destination for the operated bit. A bit from a register in the Register File can be copied into T by the `BST` instruction, and a bit in T can be copied into a bit in a register in the Register File by the `BLD` instruction.
###### Bit 5 – H: Half Carry Flag:
The Half Carry Flag H indicates a Half Carry in some arithmetic operations. Half Carry Is useful in `BCD` arithmetic.
###### Bit 4 –S: Sign Bit:
The S-bit is always an exclusive or between the Negative Flag N and the Two’s Complement Overflow Flag V.
###### Bit 3 – V: Two’s Complement Overflow Flag:
The Two’s Complement Overflow Flag V supports two’s arithmetic complements.
###### Bit 2 – N: Negative Flag:
The Negative Flag N indicates a negative result in an arithmetic or logic operation.
###### Bit 1 – Z: Zero Flag:
The Zero Flag Z indicates a zero result in an arithmetic or logic operation.
###### Bit 0 – C: Carry Flag:
The Carry Flag C indicates a carry in an arithmetic or logic operation.

#### General Purpose working registers
 
![[General purpose working registers.png]]

#### X, Y, Z registers

![[X, Y, Z registers.png]]
#### Stack 
