# [1791. Find Center of Star Graph](https://leetcode.com/problems/find-center-of-star-graph/)

There is an undirected **star** graph consisting of `n` nodes labeled from `1` to `n`. A star graph is a graph where there is one **center** node and **exactly** `n - 1` edges that connect the center node with every other node.

You are given a 2D integer array `edges` where each `edges[i] = [ui, vi]` indicates that there is an edge between the nodes `ui` and `vi`. Return the center of the given star graph.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/24/star_graph.png)

**Input:** edges = `[[1,2],[2,3],[4,2]]`
**Output:** 2
**Explanation:** As shown in the figure above, node 2 is connected to every other node, so 2 is the center.

**Example 2:**

**Input:** edges = `[[1,2],[5,1],[1,3],[1,4]]`
**Output:** 1

**Constraints:**

- `3 <= n <= 105`
- `edges.length == n - 1`
- `edges[i].length == 2`
- `1 <= ui, vi <= n`
- `ui != vi`
- The given `edges` represent a valid star graph.

```Java
class Solution {
    public int findCenter(int[][] edges) {
        
    }
}
```

# Understanding:


```
                5
                |
                |
          4---- 1 ---- 2
                |
                |
                3

         edge= [u, v]
         u != v       
```


# Solution:

## Intuition:

Star: A Centre Node/Star MUST Appear in All Edges

Vertex Common in All Edges -----> Ans

Brute Force: Check for All Edges
TC: O(E)

```
3 <= n <= 105
edges.length == n - 1

Min Vertices = 3
Min Edges Length = 2
```

----> First 2 Edge will ALWAYS Exist

## Optimised:

First 2 Edges - Find the Common Vertex - Center of STAR Graph
TC: O(1)

[A,B], [B,C]
B: Common Vertex - Center

A ---- B ---- C
B: Star

Min Edges = 2
Min Vertices = 3

Example
```
[1,2],[2,3]
-----
Ans is either 1 or 2

[1,2],[2,3]
      -----
Ans is 2 (2 is common in two edges)

```
# CODE:

```Java
class Solution {
    public int findCenter(int[][] edges) 
    {
        int starVertex = -1;

        if (edges[0][0] == edges[1][0] || edges[0][0] == edges[1][1])
            starVertex = edges[0][0]; # [0][0] = 1
        else
            starVertex = edges[0][1]; # [0][1] = 2

        return starVertex;  
    }
}
```

TC: O(1)
SC: O(1)
