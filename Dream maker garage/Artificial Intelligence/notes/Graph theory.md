A graph is a mathematical structure consisting of a set of **vertices (nodes)** and a set of **edges (arcs)** connecting these vertices. Graphs are commonly used to represent relationships in various real-world problems, such as network connections, traffic routes, social networks, etc.

Generally, a graph will contain the following components:
- **Vertex**: A unit node, step in a graph
- **Edge**: The connections between vertices. 

A graph usually cares about the **connections** **/** **edges**. It can be the case of **multiple edges** between two vertices, or even a **self-connecting** vertex. 


#### Types and definitions
**Isomorphic** graph is referred to the **same connections and construction** of a graph.
Note that the shape of the edge, or the position of the vertices does not matter, it **only regard about the connection** of two nodes. 
![[Isomorphic graph.png]]


A graph can be **directed** or **undirected**. 
Directed graph means the path / connection is **one-directional**. Sometimes also referred as di-graph. 
Undirected graph means the connection has **no direction**. 
![[undirected and directed graph.png]]

When an undirected graph is becoming directional, it is called **oriented graph**. 
![[oriented graph.png]]
When a digraph is becoming undirected, it is called **underlying graph**.  
![[underlying graph.png]]

A **neighbour** refers to the edges a node is connecting to. In di-graph, a neighbour is defined as out-degree of the node. 
![[neighbour.png]]



A **degree** of graph usually refers to the amount of neighbours or edges connected to the node. 
In a directed graph, it is further distinguished between in-degree and out-degree. 
![[degree of graph.png]]


**Weighting** is the numbering on an edge or a vertex representing the cost, significance, etc. 
![[weighted graph.png]]

**Distance** is the minimum amount of edges of traversal. If two nodes are not connected, then the distance is defined to be infinity. 
**Adjacent** is when two nodes are next to each other.
**Connected** is when a node have a finite distance. 
![[distance of graph.png]]

**Subgraph** is when **some nodes or edges have been deleted** from a graph. Note that **original graph** and **empty graph** are also subgraphs. 

![[subgraph.png]]

On contrary, **supergraph** is when **some nodes and edges have been added** into original graph. Again the original graph is a supergraph for itself. 

![[supergraph.png]]

**Induced subgraph** is when a graph retains some nodes and all connecting edges within all the retaining nodes, after removal of some nodes. 

![[induced subgraph.png]]

**Induced supergraph** is when some nodes and edges are added to a graph, but not adding any edges between existing nodes. 
![[induced supergraph.png]]



A **minor** is when a graph is **contracting some edges** / **combining some nodes**. 

![[minor.png]]

A **subdivision** is adding some nodes on the **existing edges**. 

![[subdivision.png]]

**Complement graph** is when edges are added between any two points that were **not originally connected**, and edges are removed from any two points that were **originally connected**
![[complement graph.png]]

**Reverse / Transpose graph** is when the directions in a digraph are all reversed. 

![[reverse graph.png]]

**Line graph** is when edges are seen as nodes, and connections are made with two edges are connecting to the same node (this to be said **neighbouring**). 
![[line graph.png]]

**Dual / Planar graph** is a similar idea to line graph, where instead of lines, it is plane (divided area by edges) which is regarded as nodes. 
![[planar graph.png]]




#### Data structure

##### Edge List
**Edge list** is a list of edges in a graph. 

![[Edge list.png]]
```c
// Edge list data structure
struct Edge
{
  int a, b; // Starting and ending points
};

Edge edge[4]; // Four edges for the above example

// Read in 
void edge_list()
{
  int e = 0; // number of edges
  int a, b; // A starting point and a ending point
  while (cin >> a >> b)
  {
     edge[e].a = a;
     edge[e].b = b;
    e++;
   }
}

```
The storage structure in a list is very **intuitive** and **space-saving**, yet it is **inconvenient** for algorithm - it cannot quickly find the edges a vertex is connecting to. 

##### Adjacency matrix

**Adjacency matrix** is a matrix which contains the information of the edges between vertices. The information could be the **number of edges**, or **weighting**, or any other information, between two vertices.  

![[adjacency matrix.png]]
```c
// Adjacency matrix setup 
int graph[5][5]; // Set up a two-dimensional list

// Read in
void adjacency_matrix()
{
  for (int i=0; i<5; ++i)
  for (int j=0; j<5; ++j)
  graph[i][j] = 0;

  int a, b, w; // Start, end, weight
  while (cin >> a >> b >> w)
    graph[a][b] = w;
 }
```
This matrix is useful for checking the information of an edge. But the problem is that it can **only save one entry** of data in each connection. 
It can either store node weighting or edge weighting, but not both. 
If a self-connection occurred, only one of the paths is recorded if the information is weightings. To prevent loss of information, a second matrix must be used. 

##### Adjacency List
Adjacency list is similar to matrix, only difference is in a list form. 
It reduces the memory by disregarding all the empty entries from matrix. 

![[Adjacency list.png]]
```c
// Setup
// Method 1
int list[5][100]; // 5 lists, with maximum elements of 100
int size[5]; 

// Method 2
struct Element
{
 int b;
 Element* next;
};

Element* list[5]; // 5 lists

// Method 3
vector<int> list[5];

// If more than 1 entries needs to be added, consider the following configuration:
int weight[5][100]; // A second matrix for weighting

struct Element{
  int b, w;
  Element* next;
} // A struct with multiple entries

vector<pair<int,int>> list[5]; // pairing for two integer entries
```

