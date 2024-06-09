Array types are used to hold multiple values of the same type contiguously in memory. 
Declaration:
```c
	// Method 1
	uint8_t array[10];

	// Method 2
	uint8_t array = {1, 2, 3}
```

With second method, the size of array can be further specified and more space will be allocated. The remaining values are initialised to 0. 

#### Char array
Some ways to initialise a char array is as shown:
```c
	char hello[] = "Hello world\n"
	// This is functionally equivalent to 
	char hello[] = {'H', 'e', 'l', 'l', 'o', ' ', 'w', 'o', 'r', 'l', 'd', '\n', '\0'};
```

There are some differences between using a pointer points at string literal and using an array. (Pointer method: `const char * hello = "Hello world\n"` and array method: `char hello[] = "Hello world\n"`)
Pointer method allocates a pointer points to the string literal, which is usually a constant storage such as flash that may not be able to modify the content. 
Meanwhile, array method allocates 13 bytes of SRAM, which means it can be modified later. 

Q: What is memory protection?


#### Pointers
**See also: [[Pointers]]**
Arrays and pointers work closely together in terms of memory, arrays is able to freely convert to pointers. 
Using an array where a pointer is expected will result in the array being treated as a pointer to the first value in that array, example such as:
```c
	// Method 1: pointer points to string literal
	uint8_t array[] = {1, 2, 3};
	uint8_t *b = array;
	*b = 5; // array now contains {5, 2, 3}

	// Method 2: using indexing operators
	uint8_t array[] = {1, 2, 3};
	uint8_t *b = array;
	b[1] = 4; // array now contains {1, 4, 3}
```
This is important when passing arrays to a function. Because arrays cannot be passed to functions, but pointers do. 

Recall the contiguous structure of arrays, thus the pointer arithmetic is applicable here:
```c
	uint8_t *pointer;
	int index;
	pointer[index] == *(pointer + index);
```

The address of a pointer can be changed, however, array may not. 
An operation such as the following is invalid:
```c
	uint8_t array[10];
	array++; // invalid
```



- It is undefined behaviour to access any element outside of the array's valid range. It is, however, legal to have a pointer to an element one after the end of an array, as long as it does not dereference it:
```c
	uint8_t *b = &array[len(array)];
```

#### Size of array

sizeof() operator retrieves the size of entire array (multiples of 8-bit). Example such as:
```c
	uint16_t array[] = {1000, 2000, 3000};  
    uint8_t arraysize = sizeof(array); // arraysize is 6
```

This operator is useful for iterating over an array without needing to respecify its size.
```c
	for (size_t i = 0; i < sizeof(array) / sizeof(array[0]); i++) {
	    array[i] = 0;
	}
```
 - The complier does the division beforehand, hence it is actually not a concern of the processing time on hardwares

One of the application for this approach is copying the array. 
```c
	for (uint16_t i = 0; i < sizeof(src) / sizeof(src[0]); i++){
		dest[i] = src[i];
	}

	// This is equivalent to memcpy() in <string.h>
	memcpy(dest, src, sizeof(src))
```
- Note that the source must be an actual array as using pointers always return size of pointer (which is 2 bytes)

#### Multidimensional arrays

Multidimensional arrays (also known as multiple subscript arrays) can be used for holding multi-dimensional data. Declaration:
```c
	// Method 1: specifying number of rows and columns
	uint8_t map[5][5]; // Declares a 5x5 array
	
	// Method 2: specifying number of columns
	uint8_t map[][5] = {{1, 1, 1, 1, 1},
                        {1, 0, 0, 0, 1},
	                    {1, 1, 1, 0, 1},
                        {0, 0, 1, 0, 1},
                        {0, 0, 1, 1, 1}};
```
- The data is stored contiguously in memory and the format is row-major (or first- subscript-major)
- The first five bytes of map correspond to map(0, 0) to map(0, 4), and the next byte is map(1, 0) to map(1, 4)
- Note that only the first subscript of the array can be omitted in the declaration



