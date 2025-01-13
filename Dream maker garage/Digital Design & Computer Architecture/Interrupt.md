*Interrupts are used by hardware/peripherals to let the CPU know something has happened*

Interrupts **optimize** the **CPU clock cycles** from **polling method**.  

The process of handling interrupt:
1. First, some events interrupt-worthy happen (e.g. data transferred, overflow of a timer)
2. The appropriate interrupt flag (INTFLAGS) in the peripheral is set
3. Then, if the corresponding interrupt (INTCTRL field of the peripheral) is enabled, continue onto the next step
4. If the global interrupts flag (I in SREG) is enabled, continue onto the next step
5. Finally, push PC to the stack and jump to the location corresponding with that interrupt

Interrupt vectors are located on the top of program memory, starting by 0x00 which PC will be set when microcontroller is reset. 
Interrupt vectors are located at 2 word intervals after that (0x02, 0x04, 0x06 etc.).
With this two-word interval, in practice it means that it should formatted with a jump instruction at the beginning (which is `RESET`) to jump over interrupt vectors, then feature jumps inside each interrupt vector to ISR (interrupt service routine). 

Example:
```c
ISR(PORTA_PORT_vect){

	PORTA_PIN4_CTRL |= PORT_PULLUPEN_bm | PORT_ISR_FALLING_gc; 
	// enable pull up resistor and appropriate input/sense configuration.

	// function body
	VPORTA.INTFLAGS = PIN4_bm; // Clear interrupt flag
}
```


#### Interrupt vs procedure

Previously it was introduced, that a good practice is to store the CPU state onto stack before modifying registers. 
Register 0 is considered to be pure temporary storage. Register 1 is considered to be 0 register - contains the content of zero for comparison, add, adc etc. 

Writing an interrupt is similar to writing a procedure, but any part of the CPU state changed in the routine (registers, SREG) must be restored before the procedure returns. This is because an interrupt can occur when any code is running. 
An interrupt should use `RETI` function to return to main. 

When handling the interrupt, it is possible that another interrupt is triggered within handling process. So it is important to disable / enable them at correct timing. 


#### INTFLAGS

- Each peripheral capable of producing interrupts has an INTFLAGS field that is set when the conditions for the interrupt occur (even if interrupts are disabled)
- The exact format of this field depends on the type of interrupt, but for most of them, a bit is set for the type of interrupt. 
- This can be checked to determine the **exact source** of an interrupt (e.g. PORTA has 4 buttons and other interrupt-producing peripherals, but only has 1 associated interrupt vector)
- ISR must clear the appropriate INTFLAGS bit(s) after handling the interrupt, otherwise it will be repeatedly triggered
- INTFLAGS is an unusual register - read it in the normal way, but when writing to it, it works like `OUTCLR`/`DIRCLR`, clearing the bits set by the command. So, to disable the interrupt flag for port A pin 4, `VPORTA.INTFLAGS = PIN4_bm`;


#### Synchronisation

Sometimes, if there is multicore CPU, or multi-threaded system, there raise an issue for synchronisation. 

Compiler makes optimisations, that is invisible. 
There are two examples for compiler to make the optimisation. 
1. Instruction reordering: as long as the final result is the same, compiler can reorder the instructions. 
2. Variable assumption: compiler assumes the variable is the same, without actually checking the state, if there is no code that will modify this variable within this range. 

But in multi-threading or multicore system, the global variable could be modified by another core or thread. This raise the issue that the current core isn't aware of the modification, and its (1) instruction reordering and (2) non-volatile assumption will lead to an error of the current state of particular variables. 
In order to solve this, we can have some methods to address these issues. 
1. Defining the variable as **volatile** to indicate this variable may be accessed outside of the core. 
2. The macros `cli()`and `sei()` can imply a memory barrier to insure instruction reordering does not place around them











