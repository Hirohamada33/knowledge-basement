A* algorithm extends the idea from Dijkstra's algorithm and uniform cost search, but reduce the waste of calculation. 

The basic idea of A* search is to have a heuristic weight, in addition to the initial path cost. 
**Heuristic weight** is a estimation from goal state to frontier, and it is designated for a particular search problem. Some examples could be: Euclidean distance, Manhattan distance. 
In the pancake problem, the heuristic value can be designed as the index of largest pancake that is still out of place. 


It is a combination of uniform cost search and greedy search, where:
- Uniform search orders by **path cost**, or **backward cost** $g(n)$
- Greedy search orders by **goal proximity**, or **forward cost** $h(n)$

The overall cost has a formula of 
$$f(n) = g(n) + h(n)$$
where $g(n)$ is the path cost and $h(n)$ is the heuristic cost. 


**Termination** is a question raised of, when should an algorithm ends?
It is when a goal state is dequeue, not when a goal state is enqueue
 
A* **graph** search can go wrong if the heuristic consistency is broken. Consider the following example:

![[A* graph search with heuristic inconsistency.png]]

Q: What is a heuristic value?


**Heuristic admissibility:**
The heuristic value is always lower or equal to the actual cost on a path. Not to be optimistic in estimating the 
$$h(n)\leq h^*(n)$$
where $h(n)$ is the heuristic value and $h^*(n)$ is the actual cost. 

**Heuristic consistency:**

**Optimality**: 
When the system does not follow admissibility, the optimality will not apply. 



**Comparison with Uniform cost search**


Relaxation
restrict action space
constraint 

trivial 
dominance



Loop invariant: 
This can be proved by contradiction, supposing that there is a cheaper path.
![[proof of loop invariant.png]]


A fun question of manipulating heuristic values:



##### Reference
[Introduction to A* search](https://www.redblobgames.com/pathfinding/a-star/introduction.html#graphs)
[Heuristics](https://theory.stanford.edu/~amitp/GameProgramming/Heuristics.html)
