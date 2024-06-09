```c
// Emit the declaration and initialisation
while (1) {  
	pb_state_r = pb_state; 
	// Register previous PB state pb_state = PORTA.IN
	// Sample current PB state
	
	// If multiple pushbuttons are on a single port, transitions for all 
	// pushbuttons can be calculated in parallel using bitwise operations
	pb_changed = pb_state ^ pb_state_r;  
	pb_falling = pb_changed & ~pb_state;
	pb_rising = pb_changed & pb_state;
	
	if (pb_falling & PIN4_bm) {  
		// Falling edge on PA4 (press event for active-low pushbutton)  
		// Code here should only execute once, when the pushbutton is pressed
	} 
}
```