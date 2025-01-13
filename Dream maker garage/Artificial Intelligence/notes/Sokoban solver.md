This is a documentation of project Sokoban automatic solver. It inherits the structure from class Node, search functions, and pre-defined class Sokoban. 
The theories that used in Sokoban solver mainly uses [[A* search]] with [[Heuristics]]. 


Below is the brief explanation of the logic. 

#### Project description


#### Class Sokoban
A warehouse has the following objects: 
- worker, represented as symbol `@`. If in goal, represented as `!`
- boxes, represented as symbol `$`. If in goal, represented as `*`
- targets. represented as symbol `.`
- walls, represented as `#`
- weights, stored in first row, the weight of each box in left to right order. (It could be a number or not provided)
An example of a raw warehouse text file, before processing:
`None, None`
 `#######`
 `#. #  #`
`#  $  #`
 `#. $#@#`
 `#  $  #`
 `#. #  #`
 `#######`


#### Class Nodes

#### Class Problems
