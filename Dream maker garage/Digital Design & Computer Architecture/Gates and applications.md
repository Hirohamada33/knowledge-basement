**XOR gates** are useful for comparing the two inputs 
![[截圖 2024-11-07 上午11.06.28.png]]

**XNOR** on the contrary negates the output of XOR gate. 
![[截圖 2024-11-07 上午11.05.53.png]]


##### Parity generator & checker
On both side of the data transmission, a parity generator / checker is used. 

**Parity generator**
![[截圖 2024-11-07 上午11.09.55.png]]

**Parity checker** 
![[截圖 2024-11-07 上午11.10.11.png]]

##### Enable / Disable circuit
AND gates act as enable / disable circuit
![[截圖 2024-11-07 上午11.20.24.png]]


##### Merging & Inversion circuit

**OR gates** perform **signal merging** function:
![[截圖 2024-11-07 上午11.22.12.png]]

**XOR gates** perform **selectable inversion** function:
![[截圖 2024-11-07 上午11.22.57.png]]


##### Multiplexer
The multiplexer has a boolean algebra representation of $A \cdot \overline{S} + B \cdot S$ which can be constructed by the following:  
![[截圖 2024-11-07 上午11.24.43.png]]

For a four-way multiplexer, the signal selector required $n$ bits for $2^n$ inputs. 
![[截圖 2024-11-07 上午11.31.01.png]]


An eight-way multiplexer is slightly complicated. Here 
- Enable input $\overline{E}$ (active low) provided,
- `select`lines connect first to inverters (buffering), then further inverters for building logic combinations
- Both Z and $\overline{Z}$ outputs available
![[截圖 2024-11-07 上午11.33.25.png]]


For a 16-way multiplexer, it can simply be done by having s fourth bit $S_3$ for selecting which 8-way multiplexer to use. 
![[截圖 2024-11-07 上午11.41.07.png]]


##### Demultiplexer
On the contrary, the `DEMUX` takes a single input and distributes it over several outputs. 
![[截圖 2024-11-07 上午11.49.54.png]]



##### Question #question
1. Tri-state buffer and demultiplexer, which one is more widely used? Are they doing the same usage and application? 
   Tri-state buffers and demultiplexers differ significantly in their usage frequency and applications, and they do not perform the same functions.
   Whenever possible, use dedicated demultiplexers for signal path selection, especially in high-efficiency or large-scale designs.
 .     **Tri-state Buffer**:
    - **Function**:
        - Offers three states: high (1), low (0), and high impedance (Z).
        - Used to enable multiple devices to share a common bus, ensuring only one device drives the bus at any given time.
    - **Applications**:
        - Commonly found in digital circuits for bus system design (e.g., communication between microprocessors, memory, or peripherals).
        - Prevents signal conflicts, especially when multiple components need to take turns using a shared line.
    - **Usage Scope**: Very common in designs that require bus architectures.
 .     **Demultiplexer**:
    - **Function**:
        - Routes a single input signal to one of many output lines based on selection signals.
    - **Applications**:
        - Used for signal routing, such as distributing a single signal to multiple paths in data communication.
        - Often paired with multiplexers in complex signal processing systems.
    - **Usage Scope**: Common in communication systems, signal routing, and selective data transmission.
    
2. 