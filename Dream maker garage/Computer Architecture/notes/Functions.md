**Function** is some **procedures** in programming language. It takes **an input** and gives **an output**, referred as **arguments** and **return value**, respectively. 
Declaration:
```c
return_type function_name(arguments) {
	function_body
}

// One example such as:
uint16_t square(uint8_t val){
	return val * val;
}
```


#### Arrays

###### See also: [[Arrays]]
**Arrays** cannot be used as a function argument. While the below syntax is legal, it is actually using **pointer method**. 
```c
	uint8_t function(uint8_t array[])
	
	// This is equivalent to 
	uint8_t function(uint8_t *array)

	// Better approach with determination of length of array
	uint8_t function(uint8_t *array, uint16_t len)
```


#### Arguments and return values

**Arguments** are used to **provide information to a function** when calling it. These arguments are made available to that function as **local variables**. 
For a function that does not have an argument intake, it is declared as:
```c
	uint8_t function(void)
```
Specifying void explicitly is a good approach as it generates **error message** when an argument is accidentally passed into this function. 
An old syntax is written as `uint8_t function()`, this is called **variadic function** which it takes a **variable number of arguments**. 

Return is a process that terminates the current scope of program and return to the caller with a return value. Most types, except for array, can be used as returned value. 
For a function that does not have a return value, it is declared as:
```c
	void function()
```
Although a return statement is not required, it can still be used as a terminating or breaking manner. 

#### Function prototype

Some programming language has **single-pass compilation** attribute, which means it **read from the top and compile it as it executes**. Such sequential manner may have an invalid scenario when a function is called before it set up. 

Consider the below scenarios:
- Call `a()` from `main()`, but `main()` is at the top of source file
- Call `a()` from `b()` and have `b()` call `a()`, which one of these must to be declared first
- Call `a()`, but `a()` is in a separate source file
This can be resolved by using function prototypes. 

A function prototype consists of the function's return type, name and argument types, but not the body of the function. The information is sufficient for compiler to generate the code without needing to know the body content. Then, a linker (the last step in the compilation process) will then resolve all function calls and connect them to the correct functions. 
Example such as:
```c
	void function(uint8_t, uint8_t){
	// body content
	}
```
In a function prototype, argument names are optional, but for documentation purpose it can be included. 


#### Passing by reference
**Passing by reference** is where, instead of passing a value directly to a function (passing by value), the **memory address of a variable** is passed instead. 
Consider the below scenarios:
- Functions can only return up to 1 value, so pass by reference can be used to have "output parameters"
- Some variables can be quite large, and creating copies of them undesirable, so passing a pointer avoids the need to create a copy

In C, passing by reference is achieved by using pointers. 

An example of passing by reference is `scanf()` function. 
```c
	 printf("What number should I display?\n");
     int num;
     scanf("%d", &num);
     display_decimal(num);
```

#### Stack
**Local** variables usually **do not have a fixed location in memory** and are instead stored on the **stack**, because functions can have several callings and recursions. 
The **location in the calling function** is also stored on the **stack**, so that the program counter can return there after the function finishes executing. 
In addition, the compiler will generate code to store certain registers that are modified within the function onto the stack, so that they can be restored when the function returns.

When declaring variables and arguments, this **will not increase the reported explicit SRAM usage**. However, at the moment **the function is called**, the **cost of memory incurs**.
For some architecture, including the one specific used in this course, has the structure where **fixed variables** are **stored starting from the top of SRAM** and **stack** are stored **starting from the bottom**. This implies that there could be a **memory collision** if stack incorrectly grows too tall. 
Some cases include: **recursive code**, code that declares **many large local variables**, and code that is **nested very deeply**. 