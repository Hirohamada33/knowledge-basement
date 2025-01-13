#### Pointers & dereferencing
![[Pointers]]


#### Dynamic allocated memory
**Dynamic allocated memory** refers to memory that is allocated during the runtime of a program, rather than at compile-time. This allows programs to request and manage memory as needed, making it more flexible for handling varying amounts of data. Memory is typically allocated dynamically using functions like `malloc`, `calloc`, or `new` in programming languages like C and C++.
When memory is dynamically allocated, it is stored on the **heap**. It must be explicitly released (e.g., using `free` or `delete`) to avoid memory leaks.

##### Motivation & scenario
Dynamic memory allocation is used when **the size of the data or the number of objects to be handled cannot be determined** at compile time. It allows the program to **adapt to varying inputs and optimise resource usage**.

Dynamic memory allocation is essential for programs where the **exact memory requirements are not known in advance**. Applications like databases, graphics processing, and network systems rely heavily on it for optimal performance and adaptability.
##### Advantages
1. **Flexibility**  
    Memory can be allocated and deallocated as needed, making it ideal for applications with variable or unpredictable data sizes, such as dynamic arrays or linked lists.
2. **Efficient Memory Usage**  
    **Memory is allocated only when required**, reducing wastage and making the program more efficient.
3. **Scalability**  
    Enables programs to handle **large or growing datasets without being constrained by predefined memory sizes**.
4. **Supports Data Structures**  
    Facilitates the implementation of advanced data structures like **linked lists, trees, and graphs**.

##### Drawbacks
1. **Manual Management Required**  
    Developers must explicitly deallocate memory (e.g., using `free` or `delete`), which can lead to **memory leaks** if forgotten.
2. **Overhead**  
    Dynamic allocation and deallocation have **runtime costs**, potentially making the program slower compared to stack allocation.
3. **Fragmentation**  
    Repeated allocations and deallocations can cause **memory fragmentation**, leading to inefficient use of memory.
4. **Complexity**  
    Programs using dynamic memory are harder to debug and maintain, as issues like dangling pointers and double-free errors can arise.

#### Common issues
##### Dereferencing pointers to non-allocated memory 
Dereferencing a pointer to an address that is not allocated is a common issue that leads to undefined behaviour. The same applies to dereferencing pointers to NULL. NULL is a special value for a pointer that doesn't indicate an address in memory. 
```cpp
int *p; 
a = 2
p = &2 // allowed

p = new int
*p = 3; // points to dynamic allocated memory
delete p


*p = 4; // undefined behaviour, points to unallocated memory
p = NULL; // undefined behaviour, NULL pointer

```
##### Deallocating dynamically allocated memory more than once
Deallocating dynamically allocated memory more than once causes undefined behaviour.
```cpp
int *p; 
p = new int; 
delete p;

delete p; // undefined behaviour, deallocating dynamic allocated memory more than once

```

Another example with two pointers pointing to the same dynamically allocated memory area, and deallocating more than once. 
```cpp
int *p1, *p2; 
p1 = new int; 
p2 = p1; // now p2 points to the same memory area as p1

delete p1; // deallocating once
delete p2; // undefined behaviour, deallocating same memory area again
```
##### Memory leakage
Memory leaks happen when we don't deallocate memory that has been dynamically allocated. We can't deallocate dynamically allocated memory whose address we don't have anymore.
```cpp
int *p; 

p = new int; // a new memory area has been allocated

p = new int; // another new memory area has been allocated
			 // we have lost the address of the previous one
		     // so it can't be deallocated anymore
		     // and it will remain unavailable
		     // (modern operating systems will 
		     // deallocate it at the end of the program)
```

When memory is not dynamically allocated, the memory is deallocated when variables go out of scope. 
```cpp
for(int i = 0; i < 1000; i++){
	f();
	// at each iteration variable a in f() is allocated
	// and then deallocated when the function terminates
}
```

However for dynamic allocated memory the deallocated process should be manual
```cpp
void f(){
    int *p;
    pi = new int;
}

for(int i = 0; i < 1000; i++){
	f();
	// at each iteration new memory is allocated in f()
	// but it is not deallocated when the function terminates
	// and we can't access or deallocate it anymore
}
```