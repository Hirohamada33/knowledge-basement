**Depth-first search (DFS)** is a strategy that priortises the sequence of search on **expanding the successors** (depth) then the same level of the node (breadth). 
It follows a **LIFO (Last-In-First-Out)** sequence, so the successors that just been discovered are sequenced in the beginning of the frontier list. 

In the following example, the sequence of search should be: `[A, B, C, F, G, N, O, L, ...]`

![[DFS.png]]


|                                 Complete                                  |                  Time                   |          Space          | Optimal |
| :-----------------------------------------------------------------------: | :-------------------------------------: | :---------------------: | :-----: |
| Yes (only if revisit is avoided <br>and it is not a infinite depth space) | $1+b + b^2 + b^3 + ...+b^n$<br>$O(b^m)$ | Linear space<br>$O(bm)$ |   No    |

In terms of time, the search time will be terrible if the maximum depth of the tree `m` is much larger than the depth of the shallowest solution `d`. 


```python
def dfs(graph, start):
	path = []
	stack = [start]
	while stack:
		node = stack.pop()
		path.append(node)
		if node == goal:
			return path
	for n in graph[node]:
		stack.append(n)
	return None
```

