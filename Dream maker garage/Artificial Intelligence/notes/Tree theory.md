The definition to a tree is, any two points in a tree are connected, and a tree does not contain a cycle. 
It can be considered as an expansion first on a node, and towards other nodes. Every node can only be visited once. 

#### Properties

Tree is a similar concept to graph, but different in several aspects listed below:

1. **Cycle:**
   - **Graph:** A graph can contain cycles, which are loops formed by edges that allow you to return to a node without repeating any edges.
   - **Tree:** A tree is a special type of graph that does not contain any cycles. It is a connected graph with exactly one path between any two nodes, making it acyclic.
2. **Connection:**
   - **Graph:** In a graph, nodes may or may not be directly connected to each other. There is no restriction on how the nodes are connected.
   - **Tree:** In a tree, every pair of nodes is connected by exactly one unique path. This means that there is a clear parent-child relationship between nodes, forming a hierarchical structure.
3. **Root and Parent-Child Relationship:**
   - **Graph:** A graph does not have a root or a specific parent-child relationship. Nodes in a graph can be connected in any way.
   - **Tree:** A tree has a root node from which all other nodes are reachable. Each node in a tree (except the root) has exactly one parent node and zero or more child nodes. This hierarchical structure is absent in general graphs.
4. **Direction:**
   - **Graph:** Edges in a graph may be directed or undirected, depending on whether there is a specific direction associated with each edge.
   - **Tree:** In a tree, the edges are directed, with each edge pointing from a parent node to a child node. This directed structure maintains the hierarchy of the tree.
5. **Substructure:**
   - **Graph:** A graph can have multiple disconnected components or subgraphs.
   - **Tree:** A tree is a single connected component with no disconnected substructures.

**Properties**
1. A tree does not have a cycle. If there is a cycle, it is a graph instead
2. Every node on a tree is connected to each other
3. There is only a path between two nodes
4. Adding an edge between any two nodes in a tree will result in a cycle
5. Deleting any branch in a tree will result into two trees
6. Number of branches = number of nodes - 1

#### Type and definition

**Terminology**:
- **Node**: A point on a tree (vertex)
- **Branch**: A connecting line between two nodes (edge)
- **Root**: A node without parents node. A starting node in expansion. 
- **Leaf**: A node without children nodes. An ending node in expansion. 
- **Level**: The hierarchy in node expansion. The level of a node can be different depending on which node is chosen to be the root.  
- **Forest**: One or multiple trees. Note that two or more trees are completely disconnected to each other. 

A tree is **unrooted** when there is no **labelling** or **indication** of which node is the root. 

![[rooted & unrooted tree.png]]


**Distance** is the amount of branches a node has to go through to get to another node.  
![[distance of tree.png]]

**Depth** is the distance from the root to a node. 

![[depth of tree.png]]

On contrary, **height** is the distance from a leaf to a node. 
![[height of tree.png]]

**Radius** 