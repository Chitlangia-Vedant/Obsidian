"Finite" Set of "Vertices" and "Edges" which connects a pair of vertices

```
	0----1-----2
	|    |
	|    |
	|    |
	|    |
	3----4
```

Vertices: Nodes in a Graph
Edges: Connections that connects a pair of vertices

Vertices = Nodes = V = {0,1,2,3,4}
Edges = Connections = E = {0-1, 1-2, 0-3, 3-4, 1-4}


# Types/Classification:

## (1) Based upon Weight

- Weighted: Weight/Cost is Associated with "Every" Edge in Graph

```
	  10     15
	0----1-----2
	|    |
  5 |    | 20
	|    |
	|    |
	3----4
	  7
```

- Unweighted: NO Weight/Cost is Associated with "Any" Edge in the Graph

```
	0----1-----2
	|    |
	|    |
	|    |
	|    |
	3----4
```

Ques:

```
      10    15
	0----1-----2
	|    |
	|    |
   5|    | 20
	|    |
	3----4
```


Weighted/Unweighted Graph?
- None

## (2) Based upon Direction

- Directed: One Way
- Undirected: Two Way

A ------> B: A to B Path - Directed Graph (Uni-Directional Graph)

A ------ B: A to B Path and B to A Path: UnDirected

A <-----> B: A to B Path and B to A Path: Bi-Directional Graph

Note: 
Bi-Directional Graphs behave like UnDirected Graph

UnDirected and Unweighted Graph:

```
	0----1-----2
	|    |
	|    |
	|    |
	|    |
	3----4
```

# Representations:
(1) Adjacency Matrix
(2) Adjacency List

## Adjacency Matrix

$V*V = V^2$
Rows = V
Cols = V

### Unweighted: 

Direct Path Exist,
	value = 1
else
	value = 0

### Weighted: 

Direct Path Exist,
	value = weight
else
	value = -inf/0 (Depending upon values of weights)

```
       5     10
	0----1-----2
	|    |
	|    |
	|    |
	|    |
	3----4
```

V = 5
Matrix = 5*5


```
D   0   1   2   3   4  

S
   ------------------
0 | 0   1   0   1   0     
1 | 1   0   1   0   1
2 | 0   1   0   0   0
3 | 1   0   0   0   1
4 | 0   1   0   1   0
```

(1) Diagonals are 0: adj[i][i] = 0: No Self-Edge in Graph
(2) Adjacency Matrix: Symmetrical across primary Diagonal
					  adj[i][j] = adj[j][i] - UnDirected Graph: ALWAYS

Complexity:
O(V^2)
## Adjacency List

Array of Lists
Size of Array = Number of Vertices

```
	0----1-----2
	|    |
	|    |
	|    |
	|    |
	3----4
```
Adjacency List Representation:

### Unweighted Graph:
`vertex 'a' ----> [List of Directly Connected Vertices with 'a']`

`Eg: 0 - [1, 3]`

### Weighted Graph:
`vertex 'a' ----> [List of Directly Connected Vertices with 'a' along with weight]`

`Eg: 0 - [{1,10}, {3,20}]`

Map:

```
a -> []
b -> []
```

Key: Vertex
Value: List of Vertices directly connected with Key

```
0 - [1, 3]
1 - [0, 2, 4]
2 - [1]
3 - [0, 4]
4 - [1, 3]
```

Complexity: O(V)

Pros: O(V^2) -----> O(V)
	  Adjacency     Adjacency 
	  Matrix		      List

# BFS
## CODE:

```
from collections import defaultdict

graph = defaultdict(list)

# src - []: Initial
# graph[src].append(dest): src - [dest]

def addEdge(graph, src, dest):
    graph[src].append(dest) # src -> dest

def BFS(graph, start):
    visited = [False] * (max(graph)+1)
    queue = []
    queue.append(start)
    visited[start] = True
    
    while queue: # O(V)
        curr_vertex = queue.pop(0)
        print(curr_vertex, end = " ")
        
        # Adjacency List: curr_vertex - [neighbours]
        # Iterating Over DIRECTLY CONNECTED VERTICES of curr_vertex
        for neighbour in graph[curr_vertex]: # 1 - [0,3]
            if visited[neighbour] == False:
                queue.append(neighbour)
                visited[neighbour] = True

        
addEdge(graph, 0, 1)
addEdge(graph, 0, 2)
addEdge(graph, 1, 2)
addEdge(graph, 2, 0)
addEdge(graph, 2, 3)
addEdge(graph, 3, 3)

print("BFS Starting from 2:")
BFS(graph, 2)

print()
print()
print("BFS Starting from 1:")
BFS(graph, 1)

print()
print()
print("BFS Starting from 0:")
BFS(graph, 0)

print()
print()
print("BFS Starting from 0:")
BFS(graph, 3)
```

```Output
BFS Starting from 2:
2 0 3 1 

BFS Starting from 1:
1 2 0 3 

BFS Starting from 0:
0 1 2 3 

BFS Starting from 0:
3 
```

TC: O(V+E)
SC: O(V)


## BFS for Disconnected Graph:

Disconnected Graph:
 
```
    0----1           2-----5
    |    |
    |    |
    |    |
    |    |
    3    4
```

Starting Vertex: 0
BFS OP: `[0 1 3 4 2 5]`

### Solution:

Simple BFS - Will Not Give me Answer

Modified BFS:
Simple BFS from EACH UNVISITED curr_vertex


BFS from 0:

OP: `[0 1 3 4]`

BFS from 1: NO (Already Visited)
BFS from 3: NO (Already Visited)
BFS from 4: NO (Already Visited)

BFS from 2: YES (Unvisited Vertex)

OP: `[2 5]`

```
Final Output = [0 1 3 4].append([2 5])
             = [0 1 3 4 2 5]
```

TC: O(V+E)
SC: O(V) - Storing Vertices in Queue

# Degrees in a Graph: ONLY for DIRECTED GRAPH

```
    0 -----> 1 ------> 2
   /|\      /|\
    |        |
    |        |
    |        |
    3        4
```

## InDegree:

Number of Edges for which Vertex V is the Destination
    OR
Number of Edges INCOMING/Merging Towards a Vertex

0: Indegree(0) = 1 (3 -----> 0)
1: Indegree(1) = 2 (0---->1, 4---->1)

## OutDegree:

Number of Edges for which Vertex V is the Source
    OR
Number of Edges OUTGOING/Emerging From a Vertex

0: OutDegree(0) = 1 (0---->1)
1: OutDegree(1) = 1 (1---->2)

# SPANNING TREES:

Definition:
- A Subgraph that connects ALL vertices in a Graph

Property:

Vertices: V
Edges: V-1

```
					10
				0---------1
				| \       |
				|   \     |  15
			   6|    5\   |
				|       \ |
				2--------3
					4
```

Undirected-Weighted Graph:

Vertices = 4 {0,1,2,3}
Edges = 5 {0-1, 0-2, 1-3, 2-3, 0-3}

Spanning Trees:

- (A) 0-1-3-2
- (B) 0-2-3-1
- (C) 2-0-3-1
- (D) 1-3-2-0

2-0-3:
- Subgraph: Yes
- Spanning Tree: No

Because ALL Vertices are NOT Covered

## MST (Minimum Spanning Trees):

- Minimum Spanning Tree
- Special Kind of Spaning Tree

Minimum: Weight of Spanning Tree

Weight of Spanning Tree: Sum of All Edges Weight in a Spanning Tree



```
					10
				0---------1
				| \       |
				|   \     |  15
			   6|    5\   |
				|       \ |
				2--------3
					4
```

Spanning Trees:

- (A) 0-1-3-2 = 10+15+4 = 29
- (B) 0-2-3-1 = 6+4+15 = 25
- (C) 2-0-3-1 = 6+5+15 = 26
- (D) 1-3-2-0 = 15+4+6 = 25

MST: Minimum Cost (Minimum Sum of Edges/Weight)

1. 2-3-0-1: 4+5+10 = 19
2. 1-0-3-2: 10+5+4 = 19

Q: Can a Graph have Multiple MST?
Ans: YES

Eg: Make Every Edge Weight Equal - All ST will become MST

## Algorithms:
- Kruskal (Greedy)
- Prim (Greedy)
##  KRUSKAL ALGO: (Greedy)

### Approach:

(1) Sort All Edges as per Weight # O(ElogE)
(2) Pick the Smallest Edge Weight # GREEDY
(3) Check if a Cycle is Performed

If a Cycle is performed, Ignore that Edge

Else:
Include that Edge in MST

(4) Repeat Step-2 until V-1 Edges

TC: O(ElogE) + O(V)
SC: O(V)

### DRY RUN:

OP:
MST: Minimum Cost (Minimum Sum of Edges/Weight)

MST: 2-3-0-1: 4+5+10 = 19
Min Cost = 19

```
					10
				0---------1
				| \       |
				|   \     |  15
			   6|    5\   |
				|       \ |
				2--------3
					4
```

Undirected-Weighted Graph:
Vertices = 4 {0,1,2,3}
Edges = 5 {0-1, 0-2, 1-3, 2-3, 0-3}

0-1: 10
0-2: 6
1-3: 15
2-3: 4
0-3: 5

After Sorting:
`[4 5 6 10 15]`

Spanning Tree Would Contain: V-1 Edges = 3 Edges


val = 4         Edge = 2-3        No of E: 1         W: 4                  Cycle: No

val = 5         Edge = 3-0        No of E: 2         W: 4 + 5 = 9          Cycle: No

val = 6         Edge = 0-2        No of E: 3         W: 4 + 5 + 6 = 15          Cycle: YES
------- NOT INCLDUE IN MST

val = 10        Edge = 0-1        No of E: 3         W: 4 + 5 + 10 = 19          Cycle: NO


No of Edges = 3 = V-1: STOP

MST: 2-3-0-1
Minimum Cost = 19

TC: O(ElogE) + O(V)
SC: O(V)

# Shortest Distance Algorithms: (Only for Weighted Graphs)

(1) Single Source Shortest Path
(2) All Pairs Shortest Path
## Single Source Shortest Path

"Single Source, Multiple Destinations"

Theory:

V: Vertices
S: Starting Vertex


Ans = `[d0, d1, d2, ............dv]`

1-D Array:
Size: V

d0: Shortest Distance of 0 Vertex from Source
d1: Shortest Distance of 1 Vertex from Source
d2: Shortest Distance of 2 Vertex from Source
ds: Shortest Distance of Source Vertex from Source = 0
dv: Shortest Distance of v Vertex from Source

Algorithms:
- Dijkstra
- Bellman Ford

## All Pairs Shortest Path

"Multiple Sources, Multiple Destinations"

V: Vertices
s[i]: All Vertices will become Source vertices and Destination Vertices


2D Array - Array of Arrays
```
Ans = 

[
	[d0, d1, d2, ............dv]

	[d0, d1, d2, ............dv]

	[d0, d1, d2, ............dv]

	[d0, d1, d2, ............dv]

............ V Times

]
```

Matrix: $V*V$

Algo: 
Floyd-Warshall Algo

Shortest Path: (Only for Weighted Graphs)

- Weights (City, Cost, Time.....)
- Edges (Minimum Sum of Edges to reach from src to dest)

## (1) DIJKSTRA ALGO:

- Algo: GREEDY
- Constraints: Negative Edge Weights WONT WORK

Example:

```
       10          15
	0--------> 1 -------> 2
	|                    /|\
	|                     |
	| 5                   | 18
	|                     |
   \|/                    |
    3---------------------
```

Directed and Weighted Graph:

V: 4 (0,1,2,3)
E: 4 (0-1, 1-2, 3-2, 0-3)

Source: 0

0: Min Cost = 0
1: Min Cost = 10
2: Min Cost = Min(25,23) = 23
3: Min Cost = 5

OP: `[0, 10, 23, 5]`

Change 18 to -18:

OP: [0, 10, INF, 5] - Dijkstra
OP: [0, 10, 0, 5] - Bellman Ford

### CODE:

```python
class Graph():

  def __init__(self, vertices):
    self.V = vertices
    self.graph = [[0 for column in range(vertices)]
          for row in range(vertices)]

  def printSolution(self, dist):
    print("Vertex \t Distance from Source")
    for node in range(self.V):
      print(node, "\t\t", dist[node])

  def minDistance(self, dist, visited):

    min = 1e7 # INF   # 1e7 = 10^7

    for v in range(self.V): # O(V)
      if dist[v] < min and visited[v] == False:
        min = dist[v]
        min_index = v

    return min_index


  def dijkstra(self, src):

    dist = [1e7] * self.V #1e7: 10^7 - INF
    dist[src] = 0 # src-src: 0
    visited = [False] * self.V # Mark all vertices as unvisited

    for i in range(self.V): # For All the Vertices
      u = self.minDistance(dist, visited)
      visited[u] = True
      for v in range(self.V):
        if (self.graph[u][v] > 0 and visited[v] == False and
        dist[v] > dist[u] + self.graph[u][v]):
        # dist(src, dest) > dist(src, transit) + dist(transit, dest)
          dist[v] = dist[u] + self.graph[u][v]

    self.printSolution(dist)
```

```
g = Graph(9)
g.graph = [[0, 4, 0, 0, 0, 0, 0, 8, 0],
    [4, 0, 8, 0, 0, 0, 0, 11, 0],
    [0, 8, 0, 7, 0, 4, 0, 0, 2],
    [0, 0, 7, 0, 9, 14, 0, 0, 0],
    [0, 0, 0, 9, 0, 10, 0, 0, 0],
    [0, 0, 4, 14, 10, 0, 2, 0, 0],
    [0, 0, 0, 0, 0, 2, 0, 1, 6],
    [8, 11, 0, 0, 0, 0, 1, 0, 7],
    [0, 0, 2, 0, 0, 0, 6, 7, 0]
    ]


print("Shortest Distance to All Vertices from 0 Vertex as Source: ")
print(" ")
g.dijkstra(0)
# Expected Ans: Shortest Path: [0, 4, 12, 19, 21, 11, 9, 8, 14]
```

Shortest Distance to All Vertices from 0 Vertex as Source: 
 
```Output
Vertex 	 Distance from Source
0 		 0
1 		 4
2 		 12
3 		 19
4 		 21
5 		 11
6 		 9
7 		 8
8 		 14
```

TC: O(V*E)
SC: O(V)

Optimised Code using PQ/Heap: O(V*logE)

## Bellman Ford Algo:

- Algo (Underlying Algo) : GREEDY
- Optimisation over Dijkstra: Works with Negative Edge Weights

For Negative Edge Weights:

Dijkstra: INF
Bellman Ford: Correct Ans

Graph:

```
      4       5        -3
    0----->2<-----3<-------4
    |     /|\    /|\      /|\
    |      | 3    | 2      | 2      
    ------>1------|-------- 
     -1
```

V: 5 {0,1,2,3,4}
E: 7 {0-1, 0-2, 1-2, 1-3, 1-4, 3-2, 4-3}

Edges:

0-1: -1
0-2: 4
1-2: 3
1-3: 2
1-4: 2
3-2: 5
4-3: -3

Expected OP: src: 0

0-0: 0
0-1: -1 (Direct)
0-2: 2 (0-1-2)
0-3: -2 (0-1-4-3)
0-4: 1 (0-1-4)

Expected OP: `[0, -1, 2, -2, 1]`

## CODE:

```Python
class Graph:

  def __init__(self, vertices):
    self.V = vertices # No. of vertices
    self.graph = []

  def addEdge(self, u, v, w):
    self.graph.append([u, v, w]) # Directed

  def printArr(self, dist):
    print("Vertex Distance from Source")
    for i in range(self.V):
      print("{0}\t\t{1}".format(i, dist[i]))

  def BellmanFord(self, src):

    dist = [float("Inf")] * self.V
    dist[src] = 0 # src- src: 0
    for _ in range(self.V - 1): # O(V)
      for u, v, w in self.graph: # O(E)
        if dist[u] != float("Inf") and dist[u] + w < dist[v]:
          dist[v] = dist[u] + w # transit vertex
                    
    for u, v, w in self.graph: # O(E)
      if dist[u] != float("Inf") and dist[u] + w < dist[v]:
        print("Graph contains negative weight cycle")
        return

    self.printArr(dist)


if __name__ == '__main__':
  g = Graph(5)
  g.addEdge(0, 1, -1)
  g.addEdge(0, 2, 4)
  g.addEdge(1, 2, 3)
  g.addEdge(1, 3, 2)
  g.addEdge(1, 4, 2)
  g.addEdge(3, 2, 5)
  g.addEdge(3, 1, 1)
  g.addEdge(4, 3, -3)
  # g.addEdge(0, 0, -5) - Negative Weight Cycle
  print("Shortest Distance from Vertex: 0 as Source: ")
  print(" ")
  g.BellmanFord(0)
```

```
      4       5        -3
    0----->2<-----3<-------4
    |     /|\    /|\      /|\
    |      | 3    | 2      | 2      
    ------>1------|-------- 
     -1
```
   
 V: 5 {0,1,2,3,4}
 E: 7 {0-1, 0-2, 1-2, 1-3, 1-4, 3-2, 4-3}
 
Expected OP: src: 0
0-0: 0
0-1: -1 (Direct)
0-2: 2 (0-1-2)
0-3: -2 (0-1-4-3)
0-4: 1 (0-1-4)

Expected OP:` [0, -1, 2, -2, 1]`

TC: O(V*E) + O(E)
SC: O(V)

## Floyd Warshall Algo:

- All Pairs Shortest Path
- Underlying Algo: Dynamic Programming
- OP: $V*V$ Matrix
- Each Vertex will become Source, Each Vertex will become Destination

Graph:

```
         10
    (0)------->(3)
     |         /|\
    5|          |
     |          | 1
    \|/         |
    (1)------->(2)
         3    
```

```
0: [0, 5, 8, 9]
1: [INF, 0, 3, 4 ]
2: [INF, INF, 0, 1]
3: [INF, INF, INF , 0]
```

Directed and Weight Graph:

V: 4 (0,1,2,3)
E: 4 {0-1, 0-3, 1-2, 2-3}

0-1: 5
0-3: 10
1-2: 3
2-3: 1

All Pairs Shortest Path:

OP: $V*V$ Matrix: $4*4$ Matrix

Expected OP:

```
	0    1    2   3
   -----------------
0 | 0    5    8   9
1 | INF  0    3   4
2 | INF  INF  0   1
3 | INF  INF  INF 0
```

## CODE:

```Python
V = 4
INF = 99999

def floydWarshall(graph): # O(V^3)
  dist = list(map(lambda i: list(map(lambda j: j, i)), graph))

  for k in range(V): # O(V)

    # pick all vertices as source one by one
    for i in range(V): # O(V)

      # Pick all vertices as destination for the
      # above picked source
      for j in range(V): # O(V)

        # If vertex k is on the shortest path from
        # i to j, then update the value of dist[i][j]
        dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
        # src: i, dest: j, transit: k
        # Check if direct i-j is shorter or i-k + k-j is shorter
  printSolution(dist)


def printSolution(dist):
  print("Following matrix shows the shortest distances \
between every pair of vertices")
  for i in range(V):
    for j in range(V):
      if(dist[i][j] == INF):
        print("%7s" % ("INF"), end=" ")
      else:
        print("%6d\t" % (dist[i][j]), end=' ')
      if j == V-1:
        print()


if __name__ == "__main__":

graph = [
        [0, 5, INF, 10],
        [INF, 0, 3, INF],
        [INF, INF, 0, 1],
        [INF, INF, INF, 0]
    ]

floydWarshall(graph)
```

Graph:
```
	 10
(0)------->(3)
 |         /|\
5|          |
 |          | 1
\|/         |
(1)------->(2)
	 3   
```

Expected OP:

```
	0    1    2   3
   -----------------
0 | 0    5    8   9
1 | INF  0    3   4
2 | INF  INF  0   1
3 | INF  INF  INF 0
```
																																																																																																							
TC: O(V^3)
SC: O(V)