
**Create a file object**:
```c++
	std::ifstream infile;
```

**Checking if the file is opened**:
```c++
	infile.is_open()
	
	
	// when the session is finished, the file need to be closed
	infile.close();
```
This is done in a remarkably similar way to using `std::cin`

**Passing the value**:
```c++
	// for a file that contains 4, 10 
	infile >> tmp;
	std::cout << tmp << std::endl; 
	// this will read and then print 
	// the first number in the file
	// (which in the example above is 10)
	infile >> tmp;
	std::cout << tmp << std::endl;
	// this will read and then print 
	// the second number in the file
	// (which in the example above is 4)
```

