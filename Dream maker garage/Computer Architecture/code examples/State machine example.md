```c
typedef enum {
	FULL,
	HALF,
	EMPTY
} fuel_state;

int main(void) {
    fuel_state state = FULL;

    while(1) {
        switch (state) {
            case FULL:
                if (!(PORTA.IN & PIN4_bm)){
	                //S1 pressed
	                state = HALF;
	                display('H');
                }
            break; 
            case HALF:
	            if (!(PORTA.IN & PIN4_bm)) {
                    //S1 pressed
                    state = EMPTY;
                    display('E');
                }
            break; 
            case EMPTY:
                if (!(PORTA.IN & PIN7_bm)) {
                    //S4 pressed
	                state = FULL;
	                display('F');
                }
            break;
            default:
	            state = FULL; display('F');
	    } 
	}

}
```