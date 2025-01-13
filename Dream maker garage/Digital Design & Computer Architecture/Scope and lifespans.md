Scope can be considered as "effective range" of a variable or identifier. 
Scope affects (1) **visibility** and (2) **lifespan of a variable**. It is hierarchical, which means that a variable is only visible if the current scope is inside the scope where the variable is defined. 
Variable can be declared and "hide" variable in an outer scope with the same name (the variables still exist, but cannot be seen from code running in the inner scope)

#### Global

Variables declared at the top level is referred as global variables. It can be accessed and modified anywhere within the program (unless being hidden), hence it is a good practice not to declare sensitive variables as global as it could be easily modified without knowing which block modifies it. 
Example:
```c
uint8_t global = 0;
uint8_t my_function(void){
	global += 3;
	return global;
}```

Global variables are allocated at fixed location, such as SRAM. It has unlimited lifespan.

#### Local

Local variables can only be seen within the block (or below the hierarchy). It has the lifespan that ends when the function returns. By default, local variables stored on the stack. 
Function arguments also work like local variable in terms of scope and lifespan. 
It is important to know when returning pointers, do not return a pointer to a local variable that is declared inside the function; that variable will no longer exist by the time the function returns and the pointer will be invalid. 

#### Static local

The static qualifier changes the lifespan of the local variable it applies to, but not the scope. Static local variables instead stored in fixed location in memory (SRAM), rather than on the stack. Hence, their lifespan is the duration of the program, same as global variables. 
At recursions, only one copy of a static local variable exists. 
```c
uint8_t counter(void){
	static uint8_t count = 0;
	return ++count;
}
```


#### Block scope

Block is declared by a pair of curly braces `{}`. 
Variables declared inside blocks (inside the body of `if / else` statements, `while / do-while`, `for loops` etc.) are scoped within that block and have a lifetime that ends when the block ends. 
Examples of `if / else` and `do-while` blocks:
```c
if (paused) {
    uint8_t btn = !(VPORTA.IN &PIN7_bm);
	if (btn) paused = 0;
}

// Here btn is declared within if (paused) block and ends when it finished. 

uint8_t a = 3;
do {
	uint8_t a = VPORTA.IN & PIN4_bm;
} while (a);
display_decimal(a); // Displays 3

// Here a is "hidden" within do-while block, and it became unhidden when the bock ends.  

uint8_t b = 1;
{
	uint8_t b = 2;
}

// Example of declaration of a block. Here outer b is global, inner b is local and hiding the global variable b. 
```

Variables declared in the header of `a` for loop are scoped within the for loop, but in a second block of scope outside the body of the for loop (so your loop index variable can be hidden by a variable of the same name declared in your for loop). 
```c
for (int i = 0; i < 49; i++) {
    int i = i * 2; // This 'i' shadows the outer 'i'
    display_decimal(i);
}
```

A more explicit example is shown below (in python):
```c
for i in range(5): 
	print("Outer Loop:", i) 
	
	for i in range(3): 
		print("Inner Loop")
		
	print("Back to Outer Loop:", i) 
	
print("Outside the Loops:", i)


// Outputs:
Outer Loop: 0
Inner Loop: 0
Inner Loop: 1
Inner Loop: 2
Back to Outer Loop: 2
Outer Loop: 1
Inner Loop: 0
Inner Loop: 1
Inner Loop: 2
Back to Outer Loop: 2
Outer Loop: 2
Inner Loop: 0
Inner Loop: 1
Inner Loop: 2
Back to Outer Loop: 2
Outer Loop: 3
Inner Loop: 0
Inner Loop: 1
Inner Loop: 2
Back to Outer Loop: 2
Outer Loop: 4
Inner Loop: 0
Inner Loop: 1
Inner Loop: 2
Back to Outer Loop: 2
Outside the Loops: 4
```