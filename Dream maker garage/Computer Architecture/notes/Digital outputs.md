##### Push-pull outputs
Push-pull outputs used two transistors, one can connect the output to supply voltage and the other can connect the output to ground. 
The state of the output is determined by the logic level of the input. 
*- A push-pull output stage can both source and sink current from the connected net*. 

Some supplementary notes for sink and source:
**Sink or Source**:

- In digital circuits, pins can act as either "sinks" or "sources" of current.
- A "sink" pin can absorb (or "sink") current, returning it to ground (GND), causing the pin's voltage to drop.
- A "source" pin can provide (or "source") current, causing the pin's voltage to rise.


##### High impedance outputs (with pull-up, pull down resistors) 
High impedance outputs requires two signal, an input and an "output enable signal" (OE). When output is enabled (OE = 1), then Y = input voltage level. If output is disabled (OE = 0), then Y = HiZ state. 
An output in HiZ state is regarded as **open circuitry**, i.e. it has no effect to the rest of the circuit as if it is totally disconnected. In HiZ state, the voltage at the output will be **determined by the rest of the circuit.** 
It could be the case that all the outputs on a net are all in HiZ state. HiZ state is a **floating**, **unstable** voltage level which is not ideal if the net is in HiZ state. To avoid this situation, pull-up and pull-down resistors are used to ensure that the state of the pins are **always defined**. 
The resistors typically have high resistance (k ohms range). The reason for the high resistance is that only small amount of current is able to pass through. When OE = 0, this small amount of current will pull the state of the pins on the net to **either positive supply or ground**. When OE = 1, this small amount of current is **sourced or sunk by the active device**. 

##### Open drain outputs
For previous two types of output, there could be some risks. 
1. Multiple push-pull outputs cannot be connected to the same net, otherwise there could be potential short circuit. Imagine a net connecting a push-pull outputs with high voltage level and the other with ground level, this is a short circuit scenario. 
2. With output enable, it could solve the issue. However, with wrong timing the short circuit can still happening.
Open drain outputs only have two states, HiZ state and ground state. A pull-up resistor is used to pull the net to high state. This avoid the risk of short circuit. 
