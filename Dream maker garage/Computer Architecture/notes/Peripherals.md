Microcontrollers typically include a variety of **hardware peripherals** which provide common functions. Peripherals implemented in hardware remove some of the burden of having to write software for common functions, such as timers, PWM and serial communication. 
It also allows programmer to achieve functionality that the software cannot be possible to implement alone, such as precise timing and fast response time. 

Using peripherals have several advantages:
1. Run independent of CPU (parallel structure)
2. Perform its function without software intervention
3. Not subjected to the timing constraints from software (such as execution time of instruction, clock speed of CPU)
4. The CPU can be used to perform other computations while the peripheral is busy (such as counting, transmitting data)

The **control** and **communication** with hardware peripherals is achieved by using **peripheral registers**, which CPU can access the **memory map**. Meanwhile, peripherals also typically have **direct access** to **hardware resources** including pins, clock etc. 

#### Configuration

On reset, most hardware peripherals are **disabled by default**. Hence it must be first **configured** and **enabled** before using. Once configured, it can be used in the program. 
Note that this is all done by writing appropriate values to **peripheral registers**. 
Often the code will need to service a peripheral at regular intervals, such as:
- Read bytes received via a serial interface
- Reset a timer to start counting again 
- Update the duty cycle of a PWM output

The general configuring steps is listed below:
1. Set peripheral register values to configure the peripheral in the correct mode, with the correct settings etc. (before enabling the peripheral)
2. Check the register reset values. The default settings may be OK for the application, no changes are required.
3. Enable any peripheral interrupts that will be used (make sure there is an associated ISR)
4. Enable the peripheral. 

In CAB202 several usage of peripherals are:
1. Timer
	1. TCA - frequency synthesis (driving buzzer), PWM (display brightness)
	2. TCB - periodic interrupt (event timing)
2. Analogue to digital converter
	1. ADC - reading the position of the potentiometer
3. Serial communication
	1. USART - USB-UART communication from computer
	2. SPI - driving 7-segment display 


