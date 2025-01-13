(**See also**: [[Serial communication]])
*Protocols are **agreed-upon standards** for intercommunication for machines.*

For interpretation of the data, two devices must implement the same protocol. 

**Requirements:**
- To be able to **handle the receiver** starting to receive data during transmission (as usually the underlying medium does not have any mechanism for determining if anything is listening on the other side)
  This is because there could potentially be interrupts or resets that the sender and receiver becomes asychronous. 
- To be able to **recover from errors** in the data stream (the underlying medium may have a parity protocol, but this is often insufficient)
  The error correcting such as parity bits, transmission acknowledgement can partially prevent the data from having errors. 
- To be able to engage in **flow control** (if required and not handled by the underlying communications medium)
  There could potentially be a device with limited memory or processing unit, hence sometime the transmission may need to be slowed down, which the receiver will return a flow control statement to tell the sender as a confirmation as well as the engagement of the flow control. 
- To be **simple** enough / state needs to be small enough that it **does not place a burden** on the hardware and software on each side of the link
  There could be a strong limitation that either side of the communication protocol may be simple enough that cannot handle the complex states. An example such as 7-segment as the receiver, as it is only a shift register, it does not have much memory nor processing states. 


#### Symbols
*The fundamental data type used in serial communication protocols is the
symbol*
Although **bytes** are used more commonly in describing the data bits, the length of the data bit is actually configurable and data bits length is particular to the machines in transmission. 

8-bit symbols are common, but the length is configurable. Depending on the length it may have different properties:
- Smaller symbols are more flexible; mean more symbols transmitted at a given baud rate
- Larger symbols are less flexible, but may make more efficient use of the underlying medium (more data bits compared to start/stop/parity bits)

##### Start sequences
Due to the possibility of unexpected power cycling / resets, frame/parity errors causing symbols to be skipped etc, a start sequence is used to allow identification of the start of the message. 
This can be accomplished by starting messages with a unique symbol or sequence of symbols that will not appear anywhere else, providing a synchronisation point.
If payloads need to be able to hold arbitrary sequences of symbols, escape sequences may be used. 


###### Multi-symbol start sequences
A common and simple approach is to use a single symbol as the start sequence. If the receiver is not currently processing a message, it will ignore all received symbols until the start symbol appears.

The start sequences consist of multiple symbols increase the overhead each message, but it also have the potential benefits:
1. Reduced need for escape sequences in payloads; escape sequences may still be required, but are less likely to be needed with multi-symbol start sequences
2. Reduced likelihood of data corruption resulting in other symbols being misinterpreted as start symbols
3. Increase the identifiability of the start symbols by reduce the likelihood of misinterpretation

###### Sub-symbol start sequences
Rather than dedicating a full or multiple symbols, the sub-symbol start sequence does not need to consume an entire symbol. 
A range of symbols (e.g. all symbols with the high bit set/cleared) can be set aside as symbols that will only appear at the start of a sequence. 
This reduces the start sequence overhead, but also reduces the potential range of symbols used for the rest of the message. Usually only used when messages are very short (2-4 symbols). For example, in UTF-8 encoding, the high bit is always cleared in the first byte of a sequence, while the high bit is always set in the remaining bytes

###### Escape sequence
When there is the potential for ambiguity as to whether a given symbol or sequence of symbols is a start sequence, part of a payload, or is the sentinel of a payload, escape sequences may be necessary to handle certain characters. 
This has the same overall purpose as escape sequences in the C language: e.g. because a string literal is delimited with the "character, if a literal" character needs to appear in a string literal, it is preceded by a `\`. 
However, consider the case when the device only starts receiving data immediately after the escape symbol is sent. For instance, if start sequence is `\x0f` and escape for message `\x0f` is `\x0e\x0f`, the device will misinterpret the message if it only sees `\x0f` (i.e., connect to the transmission line between `\x0e` and `\x0f`, or other potential scenarios where `\x0e` didn't send through)
For this reason, the escape sequence usually cannot contain the symbol being escaped at all. 

A common strategy is to include an alternate sequence to represent symbols when they appear in payloads or other contexts. 
For instance, if the start sequence is `0x7F`, an escape sequence of `0xFF` `0xFE` might be used to indicate a symbol of `0x7F` elsewhere in the message. This also means that if `0xFF` `0xFE` needs to be used in the message, it too will need to be escaped (e.g. `0xFF` `0xFD` could be used to replace a literal `0xFF`)

**Escape sequence example**
In the serial protocol:
`X` is the starting sequence
`\x10` is the message ID for `message`
`message` takes a byte followed by the text as the payload
`\xFF\xFE` is the escape sequence to represent `X`

**message:**
`X\x10\x0CHELLO MR FO\xFF\xFE`



#### Messages
If the information to be exchanged cannot be entirely encoded within a single symbol, the message will be used instead. In addition, if a larger quantities of information, or information of a variable length need to be passed, then the message will be used. 
A common approach is to divide the information into discrete messages. 
Notice that messages could be sent by either side. 

Messages typically contain some or all of the following features:
- A start sequence, indicating that a new message follows
- An identifier, indicating what type of message follows (for protocols with multiple messages that can be sent)
- A payload, consisting of data specific to the type of message being sent
- Some provision for escape characters (so that arbitrary data is not confused with start sequences, for example)
- A checksum or message digest, used for verifying that the message has been sent correctly
- A stop sequence, indicating that the end of the message has been reached

##### Message identifiers
Serial protocols often have multiple categories of messages that might be transmitted:
- To instruct the device on the other end to perform a particular operation
- To inform the device on the other end of some fact

Common practice is to transmit a fixed-length identifier, such that the device knows how to process the rest of the message. Different messages may have different payloads and the identifier is used so the device knows what to expect next. 
Length will depend on how many different message types are in the protocol (for example, USB uses 4 bits to indicate the message type). 

#### Payloads
When the message identifier alone is insufficient to convey all the information the device needs, a payload may need to be sent as well. 
Payloads may be short; carrying a couple of parameters needed for that type of message, or long, conveying large amounts of data. However, a long payloads have several downsides including the increase of risk of data corruption, or the start sequences are more unlikely to be detected. For this reason, a long payload is preferably to split up to different short messages. 
The payload may contain any type of data, though serial protocols may limit this to simplify processing (e.g. avoiding symbols used for the start sequence). 

##### Length strategies
Payload itself can have various fixed length, or even could be a variable length. For this reason, it is important to find a strategy to convey either the length of the payload or when the end of the payload has been reached. 

**Payload Example**
In this serial protocol:
`X` is the start sequence
`\x0E` is the message ID for `message 1`
`\x0F` is the message ID for `message 2`

**Message 1:**
`X\x0E\x7A\x4B`

**Message 2:** 
`X\x0F\x78\x53\x30\xB4\xA9\x51\xAC\x19\xE3\xC9\x97\x78\xB1\x9D\x3 E\x8F\xAC\x76\x4F\x7D\xE0\x16\x39\xD0\x3D\xBC\x88\x5C\x1A\x3D\x8 9\xD6\x9F\xEB\x1E\x15\xAF\x78\xA8\x28\x59\xF6\xB4\xE4\xE1\x65\xB 8\x45\x0D\xE8\x9A\x30\xB0\x55\x44\x97\xE5\xAA\xA4\x40\x48\x75\x91\x79`

###### Variable length payloads
This may use the strategy to send a sequence of symbols encoding the length of the payload preceding the payload. For example, an 8-bit symbol can encode lengths 0-255 (or 1-256), two 8-bit symbols can encode lengths 0-65535 (or 1-65536). 
This puts a limit on the message length, but also allows the device to know in advance how much it is going to receive (e.g. for the purposes of memory allocation).
Alternatively, send a symbol (or sequence of symbols) following the payload, to indicate that the payload has ended. This is known as a **sentinel** (e.g. terminate the payload with` 0x00`). 

**Variable length payload example**
In this serial protocol:
`X` is the starting sequence
`\x10` is the message ID
`\x00` is the terminating character

**Length symbol method:**
`X\x10\x0BHello world`

**Sentinel:**
`X\x10Hello world\x00`


#### Encoding
The choice of encodings may be a concern, depending on the communications medium, symbol length and other requirements (e.g. human-readability)
- For example, using 8-bit symbols and using all 256 available symbols may be most efficient, but it means that much of the data transferred will not be human- readable (e.g. in PlatformIO's serial monitor)
- Human-readable encoding schemes will limit the choice of symbols to those in the 32-126 range. Base64 (using A-Z, a-z, 0-9 and + /) encodes 6 bits into an 8- bit symbol. Hexadecimal notation (0-9 and A-F) only encodes 4 bits per symbol, but is more human-decodable than Base64)


#### Handshakes
A handshake is used to have the receiving side at least acknowledge what it has received. This prevents the case of "shouting into the void". 
This may need to based on the hardware having a full-  or half-duplex serial communication medium. 

##### ACK/NACK handshakes

**ACK** (short for acknowledged)
- The receiver received the message, understands its contents and will carry out the prescribed behaviour
- Upon receiving ACK, the sender knows it can move onto sending the next message

**NACK** (short for not acknowledged)
- The receiver received the message, but there was a problem with it – the message was malformed, failed its checksum or was otherwise not able to be processed correctly by the receiver
- Upon receiving NACK, the sender may resend the message, or alternatively query for more information

![[ACK NACK handshakes.png]]


##### Message verification

###### Checksum

###### Parity check


#### Serial protocol parsing
Many serial protocols are designed to be simple to parse, due to the limited hardware of typical devices using serial communication. 
However, there are concerns around timing, state and buffers that need to be considered, especially when the device needs to do multiple things at a time. For example, `scanf()` is flexible enough to parse many simple message structures, but it is a blocking function; highly undesirable in a controller that needs to be able to do other things at the same time. 

**Timing considerations**
Periodical interrupt can be used 