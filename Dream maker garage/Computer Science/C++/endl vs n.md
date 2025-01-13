```c++
std::cout << "Test line" << std::endl;
```

```cpp
std::cout << "Test line\n";
```

The varying line-ending characters don't matter, assuming the file is open in text mode, which is what you get unless you ask for binary. The compiled program will write out the correct thing for the system compiled for.

The only difference is that `std::endl` flushes the output buffer, and `'\n'` doesn't. If you don't want the buffer flushed frequently, use `\n`. If you do (for example, if you want to get all the output, and the program is unstable), use `std::endl`.



