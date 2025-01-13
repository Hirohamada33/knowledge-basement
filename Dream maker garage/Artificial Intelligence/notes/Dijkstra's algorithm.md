Dijkstra's algorithm can be used in finding the shortest path between two nodes, or lowest cost path in weighted graph. 

The basic steps:
1. Setup: **visited**, this is an array to record the visited nodes; **nodes**, this is a dictionary, to be recording the shortest path from starting to any node
2. Initialising the dictionary values (distance) to be `INF` (infinity)


```python
def dijkstra(graph, start):
	visited = []
	index = start
	nodes = dict((i, INF) for i in graph)
	nodes[start] = 0
	while len(visited) < len(graph):
		visited.append(index)
		for i in graph[index]:
			new_cost = nodes[index] = graph[index][i]
			if new_cost < nodes[i]:
				nodes[i] = new_cost
		next = INF
		for n in nodes:
			if n in visited:
				continue
			if nodes[n] < next:
				next = nodes[n]
				index = n
	return nodes
```