The UART has the following features:
- Low cost and complexity
- Simple and ubiquitous
- Asynchronous (however the sender and receiver must configured to be the same rate)
- Frame based
- Supporting full and half duplex: Separate I/O for Tx and Rx, fully independent and simultaneous. 
- Wiring: 1 or 2 wire communication, with additional reference ground. 

Frame-based format: 
A frame-based format is referred to a transmitting method by packaging the data into fixed length and format, signalled by a starting (low) bit.  

![[frame-based format.png]]

In the format, st refers to "starting bit" that indicate the start of a new information. This is low most of the time. The data is typically 8 bits, notice that from bit 5 to bit 8, it is configurable. 
P refers to parity bit, this is the error checking bit that can identify the error or missing of information. The parity test used here is even and odd parity check, 


Configuration:

![[USART0 configuration (Polled model)]]


![[USART0 configuration (Interrupt driven)]]


