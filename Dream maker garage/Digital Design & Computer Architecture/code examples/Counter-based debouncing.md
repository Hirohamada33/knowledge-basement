```c
volatile uint8_t pb_state = PIN4_bm;

// Periodic 1ms interrupt
ISR(TCB0_INT_vect){
	static uint8_t count = 20;
	uint8_t pb_input = PORTA.IN & PIN4_bm; // Get masked input in every ISR
	uint8_t pb_changed = pb_input ^ pb_state; 
	// Define change as diff between current input and state (delayed)

	if (pb_changed){
		count --;
		if (count == 0){
			// If the count has counted stable, unchanged state for 20 secs
			pb_state = pb_input;
		}
	} else {
		count = 20; // Reset counter
	}
	TCB0.INTFLAGS = TCB_CAPT_bm; // Reset TCB interrupt flags
}
```

![[Counter-based debouncing.png]]
