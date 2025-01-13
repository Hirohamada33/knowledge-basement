*Serial communication is the process sending the data one bit at a time.*

- On a microcontroller, typically it is via I/O port

Forms of serial communication can differed by (1) Protocol used and (2) Physical interface adopted. Devices must have the same protocol and physical interface to build a communication.  
- Protocol: Data arrangement, timing. 
- Physical interface: Voltage level

Terminology:
- Transmit: to send data (Tx)
- Receive: to receive data (Rx)
- Full-duplex: bidirectional communication, and can occur simultaneously. 
- Half-duplex: bidirectional communication, only one direction at a time. 
- Simplex: unidirectional communication. 
- Synchronous: communication relies on the same clock.
- Asynchronous: communication does not rely on the same clock. 

Common serial interfaces:
- [[Universal Asynchronous Serial Transmitter (UART)]]
- [[SPI]]
- [[I2C]]
- [[CAN]]
- [[I2S]]

A brief comment about asynchronous protocol: 
Although the data transmission itself is asynchronous, meaning there is no separate clock signal transmitted alongside the data, the devices **still need to be synchronised** in terms of timing. This synchronisation is achieved by setting both devices to use the **same baud rate**.
While the data transmission is not synchronised to a separate clock signal, the timing of the transmission is still **crucial for proper reception**. Both the transmitting and receiving devices need to **agree on the timing parameters**, such as the baud rate, to ensure that they sample the incoming signal at the correct times to detect the start and end of each bit.


A supplementary note about baud rate:
Baud rate refers to the **rate at which data is transmitted over a communication channel**, typically expressed in **bits per second (bps)**. It represents the number of signal changes (or symbols) per second on the communication channel. Baud rate is an essential parameter in serial communication protocols, such as UART (Universal Asynchronous Receiver-Transmitter), SPI (Serial Peripheral Interface), and I2C (Inter-Integrated Circuit), as it determines the speed at which data is sent and received between devices.
**Higher baud rates** allow for **faster data transfer**, while **lower baud rates** result in **slower but more reliable communication**, especially over longer distances or in noisy environments.


