#### Pushbuttons states

The pushbuttons have two states: Pressed and Released. This is mapped to 1 and 0 respectively. 

As detecting an input, masking and inversion are used for general active-low configuration pushbuttons. 
```c
if (!(PORTA.IN & PIN4_bm)) {
    // Pushbutton connected to PA4 is pressed
} else {
    // Pushbutton connected to PA4 is released
}}
```

##### Rising and Falling edge detection

A rising edge is when the voltage level transitioned from 0 to 1, and a falling edge is when the voltage level transitioned from 1 to 0. 
By using **EXOR**, it indicates whether there is a **transition** happened. (1 if state changed, 0 if not.) Then, by using **AND** to the current state it can be determined this transition is a **rising edge** or a **falling edge**. 
(Note that this is under an active-low configuration)

`A = Previous state, B = Current state`

|A| B | EXOR|
|:-:|:-:|:-:|
|0 |0 |0 |
|0 |1| 1|
|1| 0| 1| 
|1|1|0|
`(A XOR B) AND B  = rising edge`
`(A XOR B) AND (NOT B) = falling edge`

Example of edge detection implementation:

![[Pushbutton edge detection]]


##### Sampling and latency

By electrical standards, actuation of a pushbutton is **slow**. A fast pushbutton press is given approx. 200ms, meanwhile a singular assembly instruction has the execution time of 50 ns. 
In sampling, it must be ensured that the sampling rate is fast enough to catch the transition states in the fastest response time. 

A **latency** is referred as the delay between user input the system output. Generally, latency between **20-60 ms** is considered unlikely to affect user performance and non-perceptible. 
Factors of latency includes **choice of sampling rate**, **post-processing steps** (such as debouncing) and **system delay time**. This should all be considered. 
#### Debouncing

**Switch bounce** is an artefact of electromechanical switches. The physical contact bounces hence introducing **unclean edge** (sometimes referred as **spurious edge**) for transition. 
If a sampling is fast enough, these spurious edges will be detected and will perform errors.

![[Bouncing of electrical switch.png]]

**Debouncing** is the process of eliminating the spurious edges associated with switch bounce. This can be achieved by both software and hardware approach. Here, mainly software approach will be introduced. 

##### Interrupt-driven sampling

A periodic ISR can be setup for interrupt-based periodic sampling. 

![[interrupt-driven sampling.png]]

Decreasing the sampling can decrease the sensitivity switch bounces. However, (1) the samples can still coincide with with bounces, and (2) this introduce latency. 
So it is really a dilemma that either the latency is significant and sensitivity is low, or the spurious edges are still likely to be detected. 

##### Counter-based approach

Instead of inspecting a singular sample and immediately change the state, multiple samples can be taken and change the state only if samples remain consistent after state changed. 

![[Counter-based debouncing]]

This method is **based on interrupt-driven sampling**, it can be seen that ISR was used within the program. 
As an improvement of pure interrupt-driven sampling, it **eliminates the short bounces** successfully. 
However, this method introduces **significant latency**, and **does not improve the sensitivity** of the push event detection, hence **short-presses may be rejected** if the event interval isn't long enough.

##### Optimised debouncing – vertical counter

In the previous example, for multiple pushbuttons handling, **multiple counters must be defined**. As an improvement, this approach introduces **vertical counter** to implement bitwise approach to **decrease the number counters** and potentially **saves execution time** by **parallel structure**. 

**Standard counter**
Maximum value of counter is determined by width of integer. One integer required for each counter. Counting requires arithmetic operation (increment).
![[standard counter.png]]

**Vertical counter**
The value of the counter is stored across multiple variables. Number of counters equal to bit width of type. Maximum value of counter determined by number of variables. Counting via simple bitwise operations (in parallel).

![[vertical counter.png]]

![[Optimised debouncing - vertical counter]]


##### Optimised debouncing – reduced delay

This optimisation **decreases the latency** on **released events**, by immediately change the state at **falling edge** and **counter-based approach** for **rising edge**.

![[Optimised debouncing – reduced delay]]
