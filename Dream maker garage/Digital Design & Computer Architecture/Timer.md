**Timers** are ubiquitous in **microcontrollers** and **embedded systems**. 
Timers provide precise measurements of **elapsed clock cycles** in hardware, which is **independent** of the software and the CPU. 

Timers have broad range of uses, here are some examples:
- Generating periodic events (via an interrupt)
- Measuring time between two events
- Generating periodic signals as an output (e.g., frequency modulated or pulse width modulated.)

For implementation, most of the timers use a **clock counter** which is **incremented** (decremented) by a clock or event. More complex behaviours can then be achieved by comparing the value of this counter to some value, and taking some action if they are equal. The actions may be: 
- Reset the timer
- Stop the timer
- Generate an interrupt
- Set, reset, toggle a pin

#### Counter and capture

![[TCB counter.png]]
TCBn in **periodic interrupt mode** is the simplest form of timer. Firstly it has a **16-bit counter** `TCBn.CNT`, which is incremented by 1 each clock cycle. 
Each cycle `TCBn.CNT` is compared with the value set in `TCBn.CCMP`, which stands for **capture and compare**. If it is **equal**, then the counter is **reset** to 0. This can be used to set a **CAPT** interrupt.

Timer clock, generally is **configurable**. It is **not necessarily the same** as the **CPU** clock, often it could be a **scaled** (slower) version. 
There are: $$T_{timer} = \frac{1}{f_{timer}}$$$$T_{interval} = counts \times T_{timer}$$
where $T_{timer}$ is the period of a timer clock, $T_{interval}$ is the time elapsed.
As an example, for a 3.33 MHz clock source it is wished to define a 1 ms period. $$T_{timer} = \frac{1}{f_{timer}} = \frac{1}{3.33\times10^6} = 300\space ns$$
$$\text{TOP} = \frac{T_{interval}}{T_{timer}} = \frac{1\times10^{-3}}{300\times10^{-9}} = 3333 \space counts$$

Notice that, for a 3.3 MHz system clock with a 16-bit counter, the maximum period is only up to 20 ms. If a longer interval is required, a **prescaler** will be used. Generally a **prescaler** is used to **divide a faster clock** by some factor to produce a **slower clock** for the timer. 
However, by using a prescaler it is trading the **precision** for **longer time period**. 
**Slower clock → Longer clock period → Lower timer precision**


It should be noted that sometimes there are two modes in a timer / counter, they are **compare** and **capture**. Such as 16-bit counters in ATmega328.  

**Compare Mode:**
- Timer compares its value with a **preset** comparison value.
- Trigger an interrupt or perform specific actions when the counter **matches the comparison value**.
- Used for generating **accurate time intervals** or **periodic events**.
- Commonly employed for **PWM signal generation**, **waveform generation**, or **timer interrupts**.

**Capture Mode:**
- Timer captures timestamps or counts of **external events**.
- Records the current counter value when an **external event occurs**.
- Used for **measuring pulse widths**, **calculating time differences between events**, or **measuring the frequency of external signals**.
- Enables various **time-related calculations** or **measurements**.

#### Interrupt
(**See also:** [[Interrupt]])
A very common use for timers is to generate interrupts at fixed intervals. The benefits of using an interrupt instead of delay loop in CPU are:
1. The CPU can be doing other computations while the timer is counting
2. The period of the timer is independent of software (i.e. there is no need to count the execution time of the CPU instructions)

There are multiple interrupts can be triggered, that is capture value, counter overflow, counter equals 0, etc. 

#### Configuration
The example of configuration is shown below. 
```c
 void timer_init() {
	cli(); // Disable interrupts globally
	TCB0.CTRLB = TCB_CNTMODE_INT_gc; 
	// Configure TCB0 in periodic interrupt mode
	TCB0.CCMP = 3333;  // Set period for 1 ms (3333 clocks @ 3.33 MHz)
	TCB0.INTCTRL = TCB_CAPT_bm;  // Enable CAPT interrupt source
	TCB0.CTRLA = TCB_CLKSEL_DIV1_gc | TCB_ENABLE_bm; 
	// CLK_PER select, no prescaler (3.33 MHz)
	sei(); // Enable interrupts globally

}

int main(void) {  
	// Initialise timer TCB0 
	timer_init();

    // This programme is interrupt driven
    // main() will just idle here     
	while(1);

}

ISR(TCB0_INT_vect) {
    // This interrupt will execute every 1 ms
    // do something here    
    TCB0.INTFLAGS = TCB_CAPT_bm; // Clear interrupt flag(s)

}
```

##### Clock source
1. Choose an appropriate interval for interrupt, by carefully selecting capture value and prescaler.
2. Consider the trading off between precision and longer timer period, base on the function requirement (i.e., is the function timing-critical).

The clock source can be configured via `CTRLA`. From bit 1 to 3 `CLKSEL`will allow the prescaler, `TCA` and event to select from for the clock source

![[TCB clock source configuration.png]]

##### Mode
Timers usually have variety of modes that can drastically change their behaviour. That including when / how the timer is updated, direction of count, capture value etc. 
1. Choose the mode wished by writing to mode register. 
2. This is the first thing that should be configured.

`CNTMODE` in `CTRLB`bit 0 to 2 allows user to choose the mode for the timer. 

![[TCB mode configuration.png]]

##### Interrupt
1. Define an ISR to handle the interrupt. 
2. Enable the relevant interrupt sources in the timer peripheral
3. Ensure interrupts are enabled globally. 
4. During the process of configuring, it is a good practice to disable interrupts to prevent unintentional triggers of interrupt. 
5. During the ISR, if there are multiple interrupt sources, use appropriate masking to check which source it was from. 

`INTCTRL`has two types of interrupt enable, which is overflow and capture. The interrupt can be found in `TCBn.INTFLAGS`

![[interrupt configuration.png]]


#### Question lists
Is there any difference between TCA and TCB?
