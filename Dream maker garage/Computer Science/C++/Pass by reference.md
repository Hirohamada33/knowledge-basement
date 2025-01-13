When we pass values to parameter by copy and then change these parameters, it's like making a photocopy of a document and then altering the photocopy: this doesn't alter the original document. And in fact in many cases this is exactly what we want.

In this case, however, we want the parameters of the function to be linked to the variables in the main in such a way that any changes to the parameters are reflected also in the variables in the main. This can be achieved by **passing by reference**.

Consider the following example:
**Pass by reference**:
```cpp
void myswap(int& n1, int& n2){
 
    int tmp;
    tmp = n1;
    n1 = n2;
    n2 = tmp;
 
}
```

**Pass by copy**:
```c++
int myswap(int n1, int n2){
    int tmp;
    tmp = n1;
    n1 = n2;
    n2 = tmp;
    return n1, n2
}
```

This essentially is the same idea as using pointers: the value being pointed by the pointer is directly manipulated. 

