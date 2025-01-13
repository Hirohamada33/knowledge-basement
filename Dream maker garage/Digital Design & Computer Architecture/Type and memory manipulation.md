With the use of pointer, the address of memory location and the content stored in it can be accessed.
```c
volatile uint8_t * portb_outclr = 0x0426;
*portb_outclr = 0b0010000;
```
Volatile is an important qualifier when dealing with external hardware. The volatile qualifier declares a data object that can have its value changed in ways outside the control or detection of the compiler (such as a variable updated by the system clock or by another program). 


#### Casting

A type conversion within C is referred as casting. Casting is an explicit conversion, as it forces this type conversion. Another type conversion is implicit, which is automatically done by the compiler. However, it is considered good programming practice to use the cast operator whenever type conversions are necessary.
Casting operation is a unary operator, it has the following structure:
```
	(type_name) expression
```

One of the examples for casting is shown below:
```c
int sum = 17, count = 5;
double mean;

mean = (double) sum / count;
printf("Value of mean : %f\n", mean );

// Output:
"Value of mean : 3.400000"
```
Here the variable sum has initial type of int, then forced to convert into double so the result has the same type with mean. Notice that here the casting operator has higher precedency than arithmetic operator. 


##### Integer promotion

Integer promotion is the process by which values of integer type "smaller" than int or unsigned int are converted either to int or unsigned int.
Example:
```c
int i = 17;
char c = 'c'; /* ascii value is 99 */
int sum;

sum = i + c;
printf("Value of sum : %d\n", sum );

// Output:
"Value of sum : 116"
```
Here compiler does an integer promotion from c to Ascii numeric form of 99, then performs the arithmetic operation. 

##### Usual Arithmetic Conversion

If integer promotion is performed and the operands still have different types, then they are converted to the type that appears highest in the following hierarchy. This step is referred as Usual Arithmetic Conversion. 

![[Usual Arithmetic Conversion.png]]

As an exmple:
```c
int i = 17;
char c = 'c'; /* ascii value is 99 */
float sum;

sum = i + c;
printf("Value of sum : %f\n", sum);

// Output:
"Value of sum : 116.000000"
```
An integer promotion of char c is first performed, but as the final variable sum is a float type, hence usual arithmetic conversion applied and the compiler converts i and c into float.


##### Conversions between numeric types

Conversions between numeric type will expand or narrow the type, sometimes causing the values to be truncated if the converted doesn't support it. 
Some more in-depth cases for numeric conversions:
```c
(uint8_t) -3 // Output: 253
(int8_t) -3.45 // Output: -3, conversion from int to float will round towards 0.
```

##### Conversion between pointer types

For pointer casting, conversion between pointers will change the pointer type but will not affect the underlying data. Unless the conversion is between a void pointer and another type of pointer. 
```c
uint16_t a = 12345;
uint8_t *b = (uint8_t *) &a; // likely to contain 57, which is low(12345)
```
Here a forced conversion is performed from uint16_t pointer to uint8_t pointer. Be mindful that this type of conversion may be unportable. 

##### Conversion between pointer and numeric types

Now, for conversion between numeric type and pointer type, it has the following rules. 
1. Casting from integer to pointer will make the pointer contain the address represented by integer. 
2. Casting from pointer to integer will store the address where pointer was pointing at, in the integer. 

The conversions are not portable, but may be necessary when working on device with MMIO (memory-mapped IO). The reason for it is not portable is that (1) the address are dependent on each architecture (2) pointer type and integer type may have different depending on the platform. 
```c
volatile uint8_t *vportb_dir = (volatile uint8_t *)0x0004;
```
In this example an integer (`0x0004`) is converted into a pointer type, where it follows (1) so that this pointer now contains the address `0x0004`

##### Qualifiers
Casting can be used to add/remove qualifiers (const, volatile etc.) from pointers. Removing const then modifying the underlying variable may result in undefined behaviour. 
```c
const uint8_t a = 3;
const uint8_t *b = &a;
*((uint8_t *)b) = 4;

// Legal definition but illegal usage
```
Here the attempt is to remove the const qualifier. However, due to the hardware limitation (?), that the constant variable is stored within flash, which is not modifiable as SRAM, hence this is a not legal, not portable usage. 
##### Truncation prevention

Consider the following example:
```c
uint16_t a = 25000, b = 10000;
uint32_t c = a * b; 
// Result is truncated
```
Before the result of a multiplied by b is stored into a `uint32` type, it got truncated to the original type of uint16. Here casting is necessary for preventing this truncation:
```c
uint32_t c = (uint32_t)a*b
```

#### Structs
*- Structures are collections of variables grouped together under a common name. The variables within the structure is referred as the **members** of the structures, and may be accessed individually.*

Structs are the **program-defined aggregate type** in C. A lot of programming language developed later were inspired by struct type. 
It is declared as:
```c
struct struct_name {
	type1 type1_name;
	type2 type2_name;
	...
};
```
This will declare a new type, and can be used just as any other types (variable declarations, function signatures etc.) 
Struct is a statement, hence it must closed with a semicolon. 
Note that this is a declaration of the **structure type**, not the **instance of the structure type**. 

For declaring the instances, 
```c
struct struct_name instance0, instance1, instance2...; 
```

Some advantages of using a struct type:
- May contain **any number of members**
- Members can be **any data type**
- Allow group of related variables to be treated as a **single type**
- **Ease the organisation** of complicated data. 

Below are the examples:
```c
// New variable declaration: 'struct name' is a type, a is a variable now declared to have the type of 'struct name'
struct name a; // declaration of structure instance

// Accessing individual member in struct (using binary dot operator)
a.x = 3;
a.y = 4;

// Initialisation 
struct name a = {3, 4};

//
```
Notice that whatever the types `struct name` initially have, the compiler will allocate the same space for variable that declared to have the type of *'struct name'*. 
Another thing to point out is the initialisation must be in order with the types sequenced in struct. 

Struct can be passed into function or copy to another variable normally.
```c
struct coord a = {3, 4};
struct coord b = a;
printf("%d %d", b.x, b.y); // Output: 3, 4
```
Which means that, potentially we can use struct to (1) passing / returning arrays in functions, (2) multiple-variables-returned function. 
```c
struct name localplayers(){
	struct name loc;
	// code body
	return loc;
}
```


##### Memory layout

Structs have precisely defined rules for memory storage, defined in C standard. Because of that, structs are useful for working with specific arrangements of data, i.e., MMIO, file formats and protocols. 
Struct members are stored sequentially. Sometimes platform has alignment requirement for data  (such as `uint16_t` can only have an address that is a multiple of 2), than it adds additional padding bytes to meet the requirement. 

Examples:
```c
typedef struct VPORT_struct{
	register8_t DIR; /* Data Direction */ 
	register8_t OUT; /* Output Value */ 
	register8_t IN; /* Input Value */ 
	register8_t INTFLAGS; /* Interrupt Flags */

} VPORT_t;

// The following are macros for virtual ports
define VPORTA (*(VPORT_t *) 0x0000)
define VPORTB (*(VPORT_t *) 0x0004)
define VPORTC (*(VPORT_t *) 0x0008)

```
Recall that for each port there are offset values for each functionality: direction, output, input and interrupts etc. 

##### Anonymous structs

For a one-off usage of struct, it can be declared without a name. 
```c
struct{
	uint8_t page, line;
} record; 
record.page = 0;
record.line = 1;
```

Anonymous structs can also be used with the `typedef` keyword (introduced next) to define a struct that can be accessed through the alias name without also making it available through the `struct (structname)` type. 
```c
typedef struct{
	uint8_t x;
	uint8_t y;
} coord;


int main(void){
	coord a = {3, 4};
	printf("%d %d", a.x, a.y);
}
```


##### Struct in struct

Structs can contain other structs. Its structure, declaration, member access and initialisation are shown below:
```c
struct coord{
	uint8_t x;
	uint8_t y;
};
// First struct declaration

struct player{
	struct coord loc;
	uint8_t hp, sp;
	char name[8];
}
// First struct within the second struct

// member access and initialisation
struct player p = {{2, 6}, 100, 50, 'Tim'};
printf("%d %d", p.loc.x, p.loc.y); 


```


##### Pointers and structs

Structs can contain pointers. Structs and the member of structs can be access by using "address of" operator (&). There is also another special operator (->) that is used for accessing members of a struct through a pointer. 
```c
// Address of operator:
struct coord a = {3, 4};
int8_t *b = &a.x;
*b = 5;
printf("%d %d", a.x, a.y); // Output: 5, 4

// Arrow operator:
struct coord *ptr = &a;
ptr->x = 5; // Same as (*ptr).x
```
In the second example, `struct coord * ptr = &a` is creating a pointer that **points to the type of "struct coord"**, and its address is **where the location has the value of `a`** (defined in the first example). 

Here the pointer is used to modify the content of x, stored within a. The expression `ptr -> x` is essentially the same is `(* ptr).x`, which is dereference of ptr (which is `a`), then use the member access notation. 
The `->` operator exists because dot has higher precedence than asterisk. 

To be more clear about the declaration of pointer pointing towards structs and unions types, it is 
```c
typedef struct / union {
	[member body]
} typeName; 

typeName x; 
typeName *ptr = &x; // pointing towards the actual instance for initialisation!

// or
struct typeName *ptr = &x; // if the typedef keyword isn't used. 

// Member access
int main(void){
	ptr -> member1 = 100; 
	ptr -> member2 = 200; 
	...
} 
```

##### Bit fields
*- Bit fields are members of structures that occupy a specific numbers of the adjacent bits.*

Inside structs (and unions) bit fields can be used to specify types that take up a specific size, counted in bits. 
It has the syntax of
```
[basetype] [field name] : [length (in bit)]
```

From the above example, struct coord can be rewritten as:
```c
struct coord{
	uint8_t x : 4; // x must be 0 - 15
	uint8_t y : 4; // y must be 0 - 15 
};
```
With the use of bit fields, it **reduce the size** required for fixed-length declaration. Notice that the base type must not have smaller space than the bit field.

The following table presents the range of bit fields. This is applicable to most of the platform. 

![[Bit field ranges.png]]


Bit fields have several properties:
- **Cannot take the address of bit field member of a struct.** This is due to the fact that in C the **minimal quantity of address is written in byte**. A portion of the minimal quantity is not allowed. 
- **Cannot have an array of a bit field.** An array of a struct with bit fields in it is allowed, but the struct will always be a minimum of 1 byte in size. 
- **Name can be omitted** - this bit field will be treated as a **padding purpose**. 
	- A special case is **zero length nameless bit field** - this is alignment bit field that will **align the next bit field member with the next data word boundary** (so that it will not share data words with existing bit fields). 
- The way values are stored and the behaviour if the current data word is insufficient to hold the next bit field is all **implementation-dependent**. It could be splitting across two words, or simply perform as a zero length nameless bit field where it just jump to a new word. 

![[bit field in words.png]]
In the example above, there are 4 bit fields each has bit length of 6. The way they are packed (this is specifically on platform AVR GCC) is from first word, least significant bit. The colours indicate the way they packed together in words. 

Bit field are really **unportable**. Each platform and hardware has **different standard for bit field allocation**. 
Additionally, another drawback of using bit field is that, from the example, variable b need to be retrieved from first word and second word. For first word, it need to be masked (excluding 6 bits on right), right shifted 6 position. For second word, it need to be masked (excluding 4 bits on left) then left shifted 4 position. Then finally they can be added together and variable b is retrieved. This shows that the **performance of retrieving b can be insufficient**. 

However, in microchip university course there presented another solution of using struct, which is to access the individual members directly and assign values to them. 
```c
struct bitField{
	unsigned int firstBit = 1; 
	unsigned int ThreeBit = 3; 
	unsigned int TwoBit = 2;
	unsigned int LastTwo = 2; 
} x

int main(void){
	x.firstBit = 0; // must be 0-1 because firstBit has 1 bit-width
	x.ThreeBit = 7; // or 0b111 for 3 bits
	x.TwoBit = 2; // or 0b10
	x.LastTwo = 1; // or 0b01
}

// The final bit field will look like: 
// 01 10 111 0
// [LastTwo] [TwoBit] [ThreeBit] [firstBit] (first is LSB)
```


#### Unions
*- Unions are similar to structs, but the members of a union all share the same memory location. In essence a union is a variable that is capable of holding different types of data at different times*

Unions are fairly similar to structs, the only differences are that 
1. Its size is the **size of the largest element** within the unions, and 
2. **Only one member can be access at a time**. 

Here is an example of a union and its memory allocation. Notice that the largest member is `float` with a size of 4 bytes. So the space allocated for `x` is 32-bit (4 bytes). 
![[union memory allocation.png]]

For member `a`, it occupies the lowest byte of the union. 
![[union member a memory allocation.png]]

For member `b`, it occupies the lowest 2 bytes of the union. 
![[union member b memory allocation.png]]

For member `c`, it occupies the all bytes of the union. 
![[union member c memory allocation.png]]

Notice that when writing a value to each member, other member will be **overwritten**. For instances, writing a value to `a` will overwrites the low byte of `b` and a portion of `c`. This is the reason of only a member can be accessed each time. 

The application of the union is also to **simplify the organisation of data structure**. An example shown is the application of union on ADC reading, for a 10-bit resolution configuration. 
Usually it requires 2 registers for high and low byte of the 10-bit readings of ADC, and when retrieving it uses masking and arithmetic operation (mask, shift and add) which is a complicated procedure. 
With a union, it simplified the process by directly assign each register to a union member. 
![[union application.png]]

Declaration:
```c
union unionName{
	type1 type1_name;
	type2 type2_name;
	...
}; 
```

#### Typedef

Typedef defines type aliases, which are types that refer to another type. 
It has the declaration of form
```
typedef [original type] [alias];
```

It is useful for simplifying the attributes of a variable. Consider the following example:
```c
typedef volatile uint8_t register8_t;
```
Now the new alias variable, register8_t, has a type of uint8_t with a qualifier of volatile. By using typedef, every time register8_t is used within the code, its qualifier does not need to be specified again. 

With the structure type, the declaration is 
```c
// Declaration of type
typedef struct [structTag]{
	type1 type1_name;
	type2 type2_name;
	...
} typename

// Declaration of instances
typename instance0, instance1, instance2...; 
```

Note that with a `typedef`, the keyword `struct` is no longer required in declaring new instances. 



