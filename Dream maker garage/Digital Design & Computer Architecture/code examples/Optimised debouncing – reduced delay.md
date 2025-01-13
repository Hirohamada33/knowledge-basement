```c
volatile uint8_t pb_state; 

ISR(TCB0_INT_vect){
	// counter initialisation
	static uint8_t count0 = 0;
	static uint8_t count1 = 0;

	uint8_t pb_input = PORTA.IN; // Get input of all pins, port A in every ISR
	uint8_t pb_changed = pb_input ^ pb_state; // Get diff of input and state

	// 1. Both count0, count1 updated only if pb_changed
	// 2. For count0, it toggles each time (using NOT to switch states)
	// 3. For count1, it toggles when count0 in last step was 1
	count1 = (count1 ^ count0) & pb_changed;
	count0 = ~count0 & pb_changed;

	// Updating the pb_state when counts up to 3 or when there's falling edge
	// (pb_changed & pb_state) forces state to become 0 if any of the state
	// or changed is 0 (which is falling edge)
	pb_state ^= (count1 & count0) | (pb_changed & pb_state);

	TCB0.INTFLAGS = TCB_CAPT_bm; // Reset interrupt
}
```
![[Optimised debouncing – reduced delay.png]]