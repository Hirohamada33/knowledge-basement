##### Sequential logic
Sequential circuits exist in one of a defined number of states at any one time. They move "sequentially" through a defined sequence of transitions from one state to the next. 
The output variables depend on the state variables, either directly or in combination with the external inputs. 

##### Synchronous and asynchronous
**Synchronous**: the timing of all state transitions is controlled by common clock. A changes in all variables occur simultaneously. 

**Asynchronous**: state transitions occur independently of any clock and normally dependent on the timing of transitions in the input variables. 

**Clock** is a square wave of fixed frequency. Often, the transition occurs at one of the edges (rising or falling) of clock pulses. 

##### Flip Flop
Flip-flops are the most fundamental element of sequential circuits. 
It has the advantage of bistable, constructed using fundamental gate, 2-complementary output $Q$ and $\overline{Q}$.    
There are many types of flip-flops: **D-type**, **SR-type**, **JK-type**
![[截圖 2024-11-29 上午11.21.22.png]]
![[截圖 2024-11-29 上午11.21.39.png]]

**Asynchronously**: output can change state whenever inputs change. 
**Synchronously**: output can only change state at clock transitions (edges)
![[截圖 2024-11-29 上午11.28.38.png]]

There are delay occurring, 
Control inputs must be held stable for (a) a time $t_s$ prior to active clock transition and (b) a time $t_h$ after active clock transition. 
![[截圖 2024-11-29 上午11.30.38.png]]


###### Set-Clear type flip flop
The waveform of SC-type flip-flop. The $Q$ updates once the instructions $S$ or $C$ is asserted. Note that the instructions do not have to be asserted at the same time as the rising edge of the clock signal. 
![[截圖 2024-11-29 上午11.32.00.png]]

The concept of edge detector is to have a small delay in inverted input (sequential delay from inverter), then using an `AND` or `NAND` to detect the rising and falling edge, respectively.  
![[截圖 2024-11-29 上午11.32.35.png]]

###### D-type flip flop 
A waveform of D-type flip flop. The signal $Q$ updates every rising edge of the current input $D$
![[截圖 2024-11-29 上午11.33.53.png]]![[截圖 2024-11-29 上午11.37.29.png]]

Meanwhile a **D-latch** is when the circuitry do not have an edge detection mechanism. The shaded area is referred as **transparent latch**. The diagram below presents the waveform that when the enable is on, the output $Q$ reflects whatever waveform of input $D$, and when the enable is off, the output $Q$ latches the last output. 
![[截圖 2024-11-29 上午11.51.26.png]]


Asynchronous inputs can be used to override the value, due to the asynchronous nature that the operation is independent to clock cycles and operand can be changed at anytime. Application such as `PRESET` and `CLR`. 
![[截圖 2024-12-02 上午11.13.00.png]]
![[截圖 2024-12-02 上午11.12.33.png]]

###### Timing and stability
For the correct operation of flip-flops, data input must not change either just before or just after clock pulses. 
If data changes near the clock, the flip-flop might enter a **metastable** state, that is neither 0 or 1.
The amount of times before and after the clock pulse in which data transitions are not allowed are called setup and hold times, usually defined by manufacturer. 
![[截圖 2024-12-02 上午11.38.39.png]]



![[截圖 2024-12-02 上午11.40.43.png]]
This is an example of cascaded flip-flop. The hold time of a flip-flop is always less than the propagation delay between `CLK` and `Q`. Rising edge of CLOCK causes the data at A to go to B and data at B to go to C in example. 
B doesn't change immediately because of the propagation delay. The input to the second flip
flop is value of B just before the `CLK` rising edge i.e. B -> C; A -> B. 
Hence, this circuit shifts the data one position to the right on each clock pulse. 