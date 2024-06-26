[[A* search]]
[[Greedy algorithm]]


The idea starts with bi-direction search. A start and a goal is both expanded and queried. 
![[截圖 2024-06-17 下午6.42.14.png]]

The disadvantage for UCS (uniform cost search) was, that it explores in every direction and has no prior information about the goal location, it expands the cost contour uniformly. 

An **informed search** is where, the agent was able to observe some information (maybe implicit) from the environment. 
For an informed search, instead of the FIFO or LIFO queue that were introduced, there is priority queue. The priority queue will search the nodes with the least value of heuristics. 
A **heuristic function** is an estimation of how close a state is to a goal. Usually a heuristic function could be Manhattan / Euclidean distance in path-finding algorithm. 
