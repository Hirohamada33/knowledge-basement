A* algorithm extends the idea from Dijkstra's algorithm and uniform cost search, but reduce the waste of calculation. 

The basic idea of A* search is to have a heuristic weight, in addition to the initial path cost. 

For an informed search, instead of the `FIFO` or `LIFO` queue that were introduced, there is `priority queue`. 
The priority queue will search the nodes with the least value of heuristics. 
##### Heuristic
**Heuristic weight** is a estimation from goal state to frontier, and it is designated for a particular search problem. Some examples could be: Euclidean distance, Manhattan distance. 
In the pancake problem, the heuristic value can be designed as the index of largest pancake that is still out of place. 


It is a combination of uniform cost search and greedy search, where:
- **Uniform cost search** orders by **path cost**, or **backward cost** $g(n)$
- **Greedy search** orders by **goal proximity**, or **forward cost** $h(n)$

The overall cost has a formula of 
$$f(n) = g(n) + h(n)$$
where $g(n)$ is the path cost and $h(n)$ is the heuristic cost. 

Some critical questions:
1. What is the advantages and limitations of uniform cost search?
   **Advantages**:
   **Limitations**: 
2. What is the advantages and limitations of greedy best search?
   **Advantages**:
   **Limitations**: ![[截圖 2024-06-17 下午8.58.44.png]]
   For a greedy best search, as can be seen that for an immediate greedy strategy (lowest cost at each search). However, immediate lowest cost isn't guaranteed to be the lowest overall cost. Hence, greedy best search itself is not optimal. 



**Termination** is a question raised of, when should an algorithm ends?
It is when a goal state is dequeue, not when a goal state is enqueue. The reason was, it could been the case where the first time the goal state discovered in the queue wasn't optimal.
Terminate the algorithm when dequeuing 

##### Admissibility
Consider the following diagram. This is a case referred as the heuristic function is inadmissible, where the estimate cost is greater than the actual cost. 
![[截圖 2024-06-17 下午9.16.51.png]]

The heuristic value is always lower or equal to the actual cost on a path. Not to be optimistic in estimating the heuristic function, but underestimation is allowed
$$h(n)\leq h^*(n)$$
where $h(n)$ is the heuristic value and $h^*(n)$ is the actual cost. 

When a heuristic function is admissible, then the solution is guaranteed to be optimal. Below is the mathematical proof. 
![[截圖 2024-06-17 下午9.20.51.png]]
![[截圖 2024-06-17 下午9.21.58.png]]
![[截圖 2024-06-17 下午9.22.17.png]]
![[截圖 2024-06-17 下午9.24.38.png]]




##### Consistency
A* **graph** search can go wrong if the heuristic consistency is broken. Consider the following example:

![[A* graph search with heuristic inconsistency.png]]
For a heuristic function to be said consistent, it follows that
$$h(A) - h(C) \leq \text{cost(A to C)}$$

**Optimality**: 
When the system does not follow admissibility, the optimality will not apply. 



**Comparison with Uniform cost search**


Relaxation
restrict action space
constraint 

trivial 
dominance

Best-graph 

Loop invariant: 
This can be proved by contradiction, supposing that there is a cheaper path.
![[proof of loop invariant.png]]


A fun question of manipulating heuristic values:



##### Reference
[Introduction to A* search](https://www.redblobgames.com/pathfinding/a-star/introduction.html#graphs)
[Heuristics](https://theory.stanford.edu/~amitp/GameProgramming/Heuristics.html)
