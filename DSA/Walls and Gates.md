#  Walls and Gates

You are given a m x n 2D grid initialized with these three possible values.

`-1` - A wall or an obstacle.  
`0` - A gate.  
`INF` - Infinity means an empty room. We use the value `2^31 - 1 = 2147483647` to represent INF as you may assume that the distance to a gate is less than `2147483647`.  
Fill each empty room with the distance to its nearest gate. If it is impossible to reach a `Gate`, that room should remain filled with `INF`

Example

**Example1**

```
Input:
[[2147483647,-1,0,2147483647],[2147483647,2147483647,2147483647,-1],[2147483647,-1,2147483647,-1],[0,-1,2147483647,2147483647]]
Output:
[[3,-1,0,1],[2,2,1,-1],[1,-1,2,-1],[0,-1,3,4]]

Explanation:
the 2D grid is:
INF  -1  0  INF
INF INF INF  -1
INF  -1 INF  -1
  0  -1 INF INF
the answer is:
  3  -1   0   1
  2   2   1  -1
  1  -1   2  -1
  0  -1   3   4
```

**Example2**

```
Input:
[[0,-1],[2147483647,2147483647]]
Output:
[[0,-1],[1,2]]
```

# Solution

## Intuition  
  
Each gate (`0`) acts as a starting point.  
  
For every gate, we perform a DFS and try to update the distance of every reachable empty room.  
  
A room is only updated if we find a **shorter distance** than its current value.  
  
Initially:  
  
- Gate = `0`  
- Wall = `-1`  
- Empty room = `INF` (2147483647)  
  
As DFS spreads outward, every room stores the minimum distance found so far from any gate.  
  
---  
  
## Algorithm  
  
1. Traverse the entire grid.  
2. Whenever a gate (`0`) is found, start DFS from that gate.  
3. In DFS:  
- Check each of the four neighboring cells.  
- If the neighbor's current value is greater than:  
```  
current distance + 1  
```  
then a shorter path has been found.  
- Update the neighbor's distance.  
- Continue DFS from that neighbor.  
4. After all gates have finished propagating, every reachable room contains its shortest distance to the nearest gate.  
  
---  
  
## Dry Run  
  
Input:  
  
```  
INF -1 0 INF  
INF INF INF -1  
INF -1 INF -1  
0 -1 INF INF  
```  
  
---  
  
### Gate at (0,2)  
  
DFS updates:  
  
```  
INF -1 0 1  
INF 2 1 -1  
INF -1 2 -1  
0 -1 3 4  
```  
  
---  
  
### Gate at (3,0)  
  
DFS starts again.  
  
Some rooms receive shorter distances.  
  
```  
3 -1 0 1  
2 2 1 -1  
1 -1 2 -1  
0 -1 3 4  
```  
  
Notice that cells already having a smaller distance are **not overwritten** because of this condition:  
  
```cpp  
grid[next] > grid[current] + 1  
```  
  
So only improvements are propagated.
## CODE (Mine)

```cpp
class Solution {
public:
    /**
     * @param rooms: m x n 2D grid
     * @return: nothing
     */
    void wallsAndGates(vector<vector<int>> &rooms) {
        // write your code here
        for(int i=0;i<rooms.size();i++){
            for(int j=0;j<rooms[0].size();j++){
                if(rooms[i][j]==0){
                    solve(i,j,rooms);
                }
            }
        }
        return;
    }
    void solve(int i,int j,vector<vector<int>> &grid){
        
        if(i>0&&grid[i-1][j]>grid[i][j]+1) {
            grid[i-1][j]=grid[i][j]+1;
            solve(i-1,j,grid);
        }
        if(i<grid.size()-1&&grid[i+1][j]>grid[i][j]+1) {
            grid[i+1][j]=grid[i][j]+1;
            solve(i+1,j,grid);
        }
        if(j>0&&grid[i][j-1]>grid[i][j]+1) {
            grid[i][j-1]=grid[i][j]+1;
            solve(i,j-1,grid);
        }
        if(j<grid[0].size()-1&&grid[i][j+1]>grid[i][j]+1) {
            grid[i][j+1]=grid[i][j]+1;
            solve(i,j+1,grid);
        }
        return;
    }
};
```