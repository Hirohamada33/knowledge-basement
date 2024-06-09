A problem is defined as five components:

1. Initial state: the initial state of the problem
   `In(state)`
2. Actions: a set of actions for state s
   `Actions(In(s)) = {ActionA, ActionB, ActionC, ...}`
3. Transitions: the result of the action, or successor state 
   `Result(In(s), ActionA) = In(sA)`
4. Goal test: can be explicit set of states, or implicit property (such as `checkmate`)
   `{In(sucess_state)}`
5. Path cost: is the summation of step costs (sum of distances, number of actions executed, etc)
   `c(s, a, s')`

A solution is a sequence of actions leading from initial state to goal state. 


Some examples of search problems
1. Vacuum world state space graph
   ![[Vacuum world state space graph.png]]

2. The 8-puzzle
   ![[The 8-puzzle.png]]

3. The 8-queen puzzle
   ![[The 8-queens puzzle.png]]

4. The 8-queen puzzle, improved version
   ![[The 8-queens puzzle, improved version.png]]

5. Robotic assembly
   ![[Robotic assembly.png]]


#### Tree search 
Generally a search tree diagram will look as below. 
Several components were included in the diagram:
- **Start node**: also referred as initial state, it is the starting point of searching in a problem
- **Explored node**: the node with known information about neighbors, edges, path costs, and the cheapest path
- **Frontier**: the end of each path, these are the nodes that only contains partial information (i.e., only ancestor information, but no successor information), hence will be where the searching take place
- **Unexplored node**: the nodes with no information. They are the successors of the frontier, may contain the goal states. 

![[search tree.png]]


In a tree search, the basic idea of a tree search is to **expand the state**, that is, to generate successors of explored states or add the exploring state to the frontier. 
It is an offline, simulated exploration of the state space. 

![[tree search pseudocode.png]]



A critical problem occurring in uninformed search is the **failure of detecting repeated states**. 
It can make a linear problem become a exponential search. This is the case for Depth-first search algorithm 

![[repeated states.png]]

To prevent this problem, a way is to use a graph search instead of tree search. The basic idea is, to remember the visited states. 



#### Graph search
The idea of graph search is same to tree search: expand the unexplored nodes, and return goal state if the successors are the final goal or return a new frontier (and remove the old one) to have a new search. 
The only difference is it expand the **unvisited nodes** instead of unexplored nodes, which **prevents the revisiting of a same node**. 
Recall that from graph and tree theory, a difference between graph and tree is whether they have a cycle (i.e., allowing several paths to connect to a node). 

```python
def graph_search(problem, frontier)
  """
    Search through the successors of a problem to find a goal 
    The argument of the frontier should be an empty queue
    If two paths reach the same state, only use the first one
    Return 
	    the node of the first goal state found
	    or None is no goal state found 
    
  """
	assert isinstance(problem, Problem)
	frontier.append(Node(problem.initial))
	explored = set()
	while frontier:
		node = frontier.pop()
		if problem.goal_test(node.state):
			return node
		explored.add(node.state)
		frontier.extend(child for child in node.expand(problem)
						if child.state not in explored
						and child not in frontier)
	return None	
```


Romanian problem essentially is a graph layout, hence it could fail on a tree search condition for the recursive search.  Here consider a graph search as "expanding the unvisited node"

![[Romanian solution in graph search.png]]


#### Variety of uninformed strategies:

- **[[Breadth-first search]]**
- **[[Uniform cost search]]**
- **[[Depth-first search]]**
- **[[Depth-limited search]]**
- **[[Iterative deepening search]]**

**Summary of uninformed search algorithm performances**:

![[summary of uninformed search algorithm performance.png]]