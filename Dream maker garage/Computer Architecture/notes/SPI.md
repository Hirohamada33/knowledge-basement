The SPI has the following features:
- Simple and ubiquitous
- Synchronous system (shared clock)
- High bitrates (typically 10-20 MHz)
- Typically used for high-speed inter-IC communication (high speed, short distance)
- Full-duplex, 2-, 3-, 4- wire with additional grounding
- Typically single master, multiple clients (or slave) mode. 


SPI signalling:
SPI has the following pinout, or signal, for the communication. These are:
- CS / SS: chip select or slave select. Active low, driven low to select a specific slave device. Idles high. 
- SCLK: clock signal. Typically it is one data bit per cycle. In default mode, data is sampled at rising edge, at idles is low.
- MISO: master in slave out. Data transmitted from slave to master. Slave output is HiZ when not CS (since the data line / bus is shared among the peripherals).  
- MOSI: master out slave in. Data transmitted from master to slave.

Below shows an example of device configuration when SPI protocol is employed. 
As can be seen, a net of clock and data lines is shared among the peripherals. Chip select is the key role here - it determines which device to be enabled (client), and the entire data line will be driven based on the connection of MCU and the device. 

![[master and slave layout.png]]
Note that in SPI, it does not have a pull-up resistor, meaning the output pins can actively drive the bus to logical high and low states. 
In terms of speed, it is way faster than I2C. 

The chip select (SS / CS) and clock (SCK) are determined by the host. The chip select signal will have a transition from high to low to indicate the start of communication, and low to high to indicate the end of the communication. 

![[SPI signalling.png]]
Here raise a question: if there's only one client, and it doesn't require to be deactivated, should the chip select line be connected to GND and leave it to be active state permanently?

The answer is no. Because the SPI protocol actually require the transition (Hi -> Lo / Lo -> Hi) to indicate the start or end of a communication. 



Some remarks about SPI:
- There are no START, STOP or ACK conditions in SPI
- The CS line is used to synchronise the devices engaged in the communication
- A Hi -> Lo transition is required to initiate the communication
- When SPI transactions are complete, place the CS line inactive
##### Troubleshoot:
