**Breadth-first search (BFS)** is a strategy that priortises the sequence of search on **expanding the same level** (breadth) then the successors of each node (depth). 
It follows a **FIFO (First-In-First-Out)** sequence. 


In the following example, the sequence of search should be: `[A, B, C, D, E, F, G]`

![[BFS.png]]

In terms of its performance and properties, 

|         Complete          |                  Time                   |                 Space                  |                     Optimal                      |
| :-----------------------: | :-------------------------------------: | :------------------------------------: | :----------------------------------------------: |
| Yes (if branch is finite) | $1+b + b^2 + b^3 + ...+b^n$<br>$O(b^d)$ | keeps every node in memory<br>$O(b^d)$ | Yes, if step cost is 1<br>Not optimal in general |

**Space** is a big problem in breadth-first search. As can be seen, it requires to keep every successor nodes (i.e., all the nodes in next level) in memory, which if the branching factor is significant, the algorithm will requires lots of storage. 



```python
def bfs(graph, start):
	visited = []
	queue = [start]
	while queue:
		node = queue.pop(0)
		visited.append(node)
		neighbors = graph[node]
		for n in neighbors:
			queue.append(n)
	return visited
```

