Usually in a compiler, for specific addressing like **assigning value to a particular register** in assembly, is **automatically done with compiler**. Hence usually it will not be a concern of which register or location to be assigned to. 
However, sometimes it could be useful for knowing and modifying the address and its value. This case a pointer can be used. 

Q: What's the difference between *store indirect* and *store direct*? 

Q: Why do I need to declare the size of pointer before accessing that address? 
For declaration of pointer:  
```c 
	uint8_t *ptr = address;
```

The pointer is declared with an asterisk, the uint8_t is the value type of this pointer is pointing to (it isn't the pointer type). Here the pointer contains an address, in this specific architecture is 16-bit. This is similar to X, Y, Z registers (which consumes two 8-bit register and contains a memory address)

If a pointer is pointing at a variable, this is referred as *referencing* that variable. 

Address-of operator (&) can be used for getting the address of the value. It is a unary operator (consist of only one operand). 
Example such as:
```c
	uint8_t *ptr = &value
```

- volatile is important for interfacing memory-mapped I/O registers. 
Q: Why is that?
A: Because usually in a multi-thread, or a variable whose value could be affected by external factor, the optimisation made by the compiler will simply do not see it and assume it is a constant variable (except defined as volatile). By assigning the volatile attribute to it, it make sure the compiler aware, or more specifically, double-check every time when the variable is used. 
#### Dereference

Once a pointer has successfully pointed to a variable (or more specific, the location or address of the variable), then this variable can be accessed and modified by using the pointer. This is called dereferencing. 
Example such as:
```c
	uint8_t a = 123;
	uint8_t *b = &a; // ptr b is pointing at location of a (or simply variable a)
	uint8_t c = *b; // c now contains the value of the variable ptr b is pointing at
	*b = 0; // ptr modifies the content of a to become 0, but c is unchanged.
```


#### Strings

String is a sequence of characters (chars), terminated with a char of value 0. It adds this terminating literal automatically. 
```c
	const char *hello = "Hello world\n";
	printf(hello)
```

In this example, the compiler sets up 13 bytes for storing the string` "Hello world\n"` -- 11 byte for "Hello world", 1 for `\n` and 1 for the terminating `\0`. 
The pointer "hello" points to the first char of the string "H", and printf() will find the following chars and know to end when it meets `\0`. This is based on the fact that the characters are contiguous in the memory. 

#### Qualifiers

Qualifiers can be used to specified pointer as well. Typical use is for the qualifier to apply to the value that is being pointed to, not the pointer itself. 
```c
	const uint8_t a = 100, c = 50;
	const uint8_t *b = &a; // Pointer to constant uint8
	b = &c; // The pointer itself can change
```

In the example above, the address pointer b is pointing to can be changed to another address, since pointer itself isn't a constant. However, the content in pointer cannot be dereferenced as the variable a and c are constant. 

Another way to understand the idea of putting qualifier before pointer is, consider it is not "assigning attribute" but "informing value type". It tells the pointer to locate the specific part of memory. 

A general rule: a pointer can add additional qualifiers (e.g. constness) but cannot take them away. (Applies to volatile too.)
Example of a legal usage and an illegal usage:
```c
	# Legal usage
	uint8_t a = 33;
	const uint8_t *b = &a; // adding constness

	# Illegal usage
	const uint8_t a = 15;
	uint8_t *b = &a; // taking constness away
```

Q: How does adding the constness to a variable affect the location of the memory?
Instead of putting into the regular variable space, SRAM, constant variable will be instead located at flash. Flash cannot be modified. 

It was mentioned that the usage of const is applying to the variable which pointer is pointing to. If it is wished to apply the qualifiers to pointers, it is as shown:
```c
	// Constant pointer points to a non-const variable
	uint8_t a = 33, b = 15;
	uint8_t * const c = &a; 

	// Constant pointer points to a constant variable
	const uint8_t * const b = &a;	

	// When a pointer is const, the following operation is illegal
	c = &b;
```


#### Point to pointer

Pointers can also be pointed to another pointers:
```c
	uint8_t a = 1;
	uint8_t *b = &a; // Pointer to uint8_t
	uint8_t **c = &b; // Pointer to pointer to uint8_t
```

And modify the address in a pointer indirectly:
```c
	uint8_t a = 1, b = 2;
	uint8_t *c = &a;
	uint8_t **d = &c;
	*d = &b; // c now points to b
```

Pointers to pointers and beyond is permitted. 

Qualifiers in pointing to pointers are also permitted, shown as follow:
```c
	const uint8_t **a; // Ptr to ptr to const uint8_t
	uint8_t * const *a; // Ptr to const ptr to uint8_t
	uint8_t ** const a; // Const ptr to ptr to uint8_t
	const uint8_t * const * const a; // const ptr const ptr to const uint8_t

```

#### Arithmetic

Arithmetic operations in pointers affect the address. A main difference compared to normal arithmetic and is pointer arithmetic works in multiples of the size of the underlying type. 
An example as shown:
```c
	volatile uint16_t *ptr = 0x0420;
	ptr += 2; // Now points to 0x0424
	// Rule: address += [ptr value * (sizeof(value) / 8)]
```
This arithmetic rule is important for working with array. 
#### Void

Sometimes it is the case that a pointer is pointing to an unknown type of variable. Using void type can handle this situation. 
Several points about void pointers:
- have no underlying type 
- cannot be dereferenced
- pointers of other types can be converted to/from void pointers without casting
- it is an illegal to convert a void pointer into a pointer that is not the same as the original type the pointer had
Example of a void pointer declaration:
```c
	int a = 33;
	void *ptr = &a;

	// Illegal usage because a has integer type
	char *ptr = &a;
```

It can be seen that after the void ptr declaration, it is not permitted to change this pointer type to any type other than int. 