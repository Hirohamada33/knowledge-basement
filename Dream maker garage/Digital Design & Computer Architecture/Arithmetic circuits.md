##### Sign magnitude
**Sign magnitude** is given by using the the `[0:6]` bits as magnitude and `[7]`as sign. Here, an example is given that by changing the sign bit, the number representation changes from +27 to -27. 
![[截圖 2024-11-18 上午11.26.55.png]]

However, the disadvantage is that (1) it needs to handle sign and magnitude separately (2) there are redundant representation of zeros (`10000000` and `00000000`). 

##### **One's complement**
**One's complement** inverts all bits to negate a number. 
![[截圖 2024-11-18 上午11.30.54.png]]

The disadvantage is that (1) it is not convenient for arithmetic, (e.g. adding 27 to -27 results in 1111 1111), and (2) redundant representation of zeros. 


##### Two's complement
**Two's complement** negates a number by inverting all bits and adding one. ![[截圖 2024-11-18 上午11.32.57.png]]
Consider this as the solution to the redundant zeros, where all the value representation was added by one and now `00000000`represents zero and `10000000`represents -1. 

A summary note of signed and unsigned number. 
![[截圖 2024-11-22 上午11.27.44.png]]
##### Sign extension
![[截圖 2024-11-22 上午11.23.05.png]]

##### Multiplication and division
The operation of left / right shifting will allow the number be multiplied / divided by $2^n$ with $n$ bits shift. 



##### Ripple carry adder and lookahead carry adder
A ripple carry is the basic implementation of multiple full adders to achieve multiple bits addition. The data input is parallel.  
![[截圖 2024-11-22 上午11.40.35.png]]
However, one main disadvantage of ripple carry adder is the propagation delay introduced. Notice that the carry only generates once the previous addition block finishes execution, causing the next full adder be standing by. 
The propagation time is equal to the propagation delay of each adder block, multiplied by the number of adder blocks in the circuit. 

A **look-ahead carry adder** 
![[截圖 2024-11-22 上午11.45.47.png]]
The `XOR` gate can be seen as a odd / even number counter. 
For `S`, the first `XOR` sees if `A`, `B` will both be the same (i.e., if the input number of `1` is odd). The second `XOR` sees if the output of first gate and Cin have the same input (i.e., if the input number of `1` is odd).  If even, it outputs 0; if odd, it outputs 1. 
For `Cout`, a carry is needed when any two of the three inputs is 1. 

| A   | B   | Cin | S   | Cout |
| --- | --- | --- | --- | ---- |
| 0   | 0   | 0   | 0   | 0    |
| 0   | 0   | 1   | 1   | 0    |
| 0   | 1   | 0   | 1   | 0    |
| 0   | 1   | 1   | 0   | 1    |
| 1   | 0   | 0   | 1   | 0    |
| 1   | 0   | 1   | 0   | 1    |
| 1   | 1   | 0   | 0   | 1    |
| 1   | 1   | 1   | 1   | 1    |

##### Comparator

##### Decoder
