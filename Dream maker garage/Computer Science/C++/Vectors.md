A vector is just a **collection of variables** of the **same type** that are contained in adjacent memory cells. It is the most used data structure in C++. 

###### Declaration:
The following code declare a variable of type vector named `v`
```c++
	std::vector<int> v;
```

###### Append an item:
The code will append the value `10` at the end of the vector. 
```c++
	v.push_back(10);
```

###### Print values:
The vector items can be printed using indexing. However, the entire vector cannot be printed at once. 
```cpp
	std::cout << v[0] << std::endl;
	// print first item in v
	
	std::cout << v.size() << std::endl;
	// print the size of the vector
	
	std::cout << v << std::endl;
	// don't work 
```


#### Function passing
In terms of syntax, vectors can be passed to functions like the other types, and vectors can be returned from functions. 
In the past different approaches were introduced and encouraged because of efficiency reasons. Later on, because of changes to the language and to compiler implementations, the matter became even more controversial (a discussion [here](https://stackoverflow.com/questions/21605579/how-true-is-want-speed-pass-by-value "https://stackoverflow.com/questions/21605579/how-true-is-want-speed-pass-by-value")). 

**By copy**
```c++
double sum_vector(std::vector<double> vin){
    double sum = 0;
    for(int i = 0; i < vin.size(); i++){
        sum = sum + vin[i];
    }
    
    return sum;
}
```
Copying all the elements of a vector into a different vector can have a relatively high computational cost, meanwhile the action of copying seems unnecessary when the user only want to view the elements. 

**By reference**
```c++
	double sum_vector(std::vector<double>& vin){}
```
Passing by reference can save much more storage, with the trade-off of security - now the values stored cannot be sure that it hasn't been modified. 

**By const reference**
```c++
	double sum_vector(const std::vector<double>& vin){}
```
In order to keep the efficiency advantage without this drawback, it can be specified that the vector, although passed by reference, is not meant to be changed. In order to do so, it is passed **by const reference**. 

This was the idiomatic way of passing vectors in input to functions in `C++` up to `C++11`. After `C++11` updates in the semantics of the language and in compiler implementations and optimisations have arguably made this less necessary and it has become more widespread to just pass “by copy” (relying on the compiler to avoid making actual unnecessary copies).


##### Function output
Similarly, the output of vectors follows the same consideration. 

**Pass by copy**
```c++
std::vector<int> from_1_to_n(int n){
    std::vector<int> vout;
    for(int i = 0; i < n; i++){
        vout.push_back(i+1);
    }
    return vout;
}
```


**Pass by reference**
```c++
void from_1_to_n(int n, std::vector<int>& vout){
    for(int i = 0; i < n; i++){
        vout.push_back(i+1);
    }
}
```