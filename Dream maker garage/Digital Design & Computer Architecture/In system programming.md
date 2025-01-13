**In-system programming (ISP)**, or also called **in-circuit serial programming (ICSP)**, is the ability of some programmable logic devices, microcontrollers, chipsets and other embedded devices to be **programmed while installed in a complete system**, rather than requiring the chip to be **programmed prior** to installing it into the system. 

There is **no general protocol** for ISCP. Although almost every microcontroller chip supports the feature, each device and manufacture has implemented it in their own protocol. Generally, the pin number is wished to be reduced. In some application, the pin number has been reduced to only 1 pin while it can also ranges from 4 - 11 pins for **JTAG** implementation. 

Typically, chips supporting ISP have internal circuitry to generate any **necessary programming voltage** from the system's normal supply voltage, and communicate with the programmer via a **serial protocol**. Most programmable logic devices use a **variant of the JTAG protocol** for ISP, in order to facilitate easier integration with automated testing procedures. Other devices usually use proprietary protocols or protocols defined by older standards. In systems complex enough to require moderately large **glue logic**, designers may implement a JTAG-controlled programming subsystem for non-JTAG devices such as **flash memory** and microcontrollers, allowing the entire programming and test procedure to be accomplished under the control of a single protocol.

#### History
In the 1990s there was some revolutionary embedded solutions for in system programming. They were OTPs (One Time Programmable, or PROM)  and EPROM (Erasable Programmable Read-Only Memory). 


Some common ISCP protocol:
- SPI (Serial Peripheral Interface)
- JTAG (Joint Test Action Group)
- ICSP (In-circuit system programming)
- SWD (Serial Wire debug)

