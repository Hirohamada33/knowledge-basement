
#### Definition
1. Input: An algorithm must have zero or more inputs
2. Finite: An algorithm must have finite amount of steps
3. Clarity: An algorithm must be described clearly
4. Effectiveness: An algorithm must be feasible to return a correct answer 
5. Output: An algorithm must have one or more outputs


#### Time complexity
The performance of an algorithm generally can be quantitatively assessed, and one of most important measurement is time complexity. 
Time complexity is expressed as a function $O(n)$. It describes how the runtime of an algorithm grows as the input size increases.
When `n` is sufficiently large, we have:
$$O(1)<O(\log n)<O(n)<O(n^2)<O(2^n)$$
- $O(1)$: Constant time complexity, indicating that the algorithm's runtime **is not affected by the size of the input**.
- $O(\log n)$: Logarithmic time complexity, indicating that the algorithm's runtime **grows logarithmically** with the size of the input.
- $O(n)$: Linear time complexity, indicating that the algorithm's runtime is **directly proportional** to the size of the input.
- $O(n \log n)$: **Linearithmic time complexity**, commonly seen in **sorting algorithms** like quick sort and merge sort.
- $O(n^2)$: Quadratic time complexity, indicating that the algorithm's runtime **grows quadratically** with the size of the input.
- $O(2^n)$: Exponential time complexity, indicating that the algorithm's runtime **grows exponentially** with the size of the input, often due to **recursive** or **combinatorial** problems.

#### Space complexity
Space complexity refers to the amount of memory space required by an algorithm to execute as a function of the input size. Similar to time complexity, space complexity is also commonly expressed as $O(n)$. It measures how the memory usage of an algorithm scales with the size of the input.

The space complexity of an algorithm can depend on various factors, including the size of the input data, the number of variables and data structures used, and any additional auxiliary space required for temporary storage.

It follows the same order from constant to exponential complexity. 
An important remind: often between time and space complexity, **time complexity has the priority**. 

