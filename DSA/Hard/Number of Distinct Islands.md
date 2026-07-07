# Number of Distinct Islands

Given a non-empty 2D array `grid` of 0's and 1's, an **island** is a group of `1`'s (representing land) connected 4-directionally (horizontal or vertical). You may assume all four edges of the grid are surrounded by water.

Count the number of **distinct** islands. An island is considered to be the same as another if and only if one island has the same shape as another island (and not rotated or reflected).

Notice that:

```
11
1
```

and

```
 1
11
```

are considered different island, because we do not consider reflection / rotation.

The length of each dimension in the given `grid` does not exceed `50`.

Example

**Example 1:**

```
Input: 
  [
    [1,1,0,0,1],
    [1,0,0,0,0],
    [1,1,0,0,1],
    [0,1,0,1,1]
  ]
Output: 3
Explanation:
  11   1    1
  1        11   
  11
   1
```

**Example 2:**

```
Input:
  [
    [1,1,0,0,0],
    [1,1,0,0,0],
    [0,0,0,1,1],
    [0,0,0,1,1]
  ]
Output: 1
```

```cpp
class Solution {
public:
    /**
     * @param grid: a list of lists of integers
     * @return: return an integer, denote the number of distinct islands
     */
    int numberofDistinctIslands(vector<vector<int>> &grid) {
        // write your code here
    }
};
```
# Approach

## Algorithm

Consider the following example, the two islands in the first figure might look identical but they are rotated so you can’t say they are the same, hence 2 distinct islands; whereas in the second figure, both islands are the same so only 1 distinct island; resulting in overall 3 distinct islands.  
Depending on the shape of the island formed, we count the number of islands. To identify distinct islands, we must track the shape of each island. Two islands are identical if the relative structure are the same, even if they appear at different places on the grid.  
  
The main idea to store shape is to store the pattern of movement (or coordinates relative to starting point) in a set while doing DFS, and use that to determine if we’ve seen this shape before. To figure out if the coordinates stored in the set data structure are identical, we can call one of the starting points a base, and subtract the base coordinates from the land’s coordinates (Cell Coordinates - Base coordinates). Now the list will be similar as illustrated.

- Traverse the entire grid using nested loops.
- When an unvisited land cell (1) is found, start a DFS from that cell.
- In the DFS, mark all connected land cells as visited.
- While traversing the island, record the relative position of each land cell with respect to the starting cell.
- Store the full list of relative positions as the island's shape.
- Insert this shape into a set to ensure only distinct shapes are counted.
- After completing the grid traversal, return the size of the set as the number of distinct islands.

```image-slider
![[Pasted image 20260705121523.png]]
![[Pasted image 20260705121530.png]]
![[Pasted image 20260705121758.png]]
![[Pasted image 20260705121806.png]]
![[Pasted image 20260705121814.png]]
![[Pasted image 20260705121821.png]]
![[Pasted image 20260705121827.png]]
![[Pasted image 20260705121833.png]]
![[Pasted image 20260705121838.png]]
```
## CODE
```cpp
#include <bits/stdc++.h>
using namespace std

class Solution {
public:
    // DFS traversal
    void dfs(int row, int col, int baseRow, int baseCol,
             vector<vector<int>>& grid, vector<vector<int>>& vis,
             vector<pair<int, int>>& shape) {
        // Mark as visited
        vis[row][col] = 1;
        // Store relative position
        shape.push_back({row - baseRow, col - baseCol});

        // All 4 directions
        int drow[] = {-1, 0, 1, 0};
        int dcol[] = {0, 1, 0, -1};

        // Explore all directions
        for (int i = 0; i < 4; i++) {
            int nrow = row + drow[i];
            int ncol = col + dcol[i];

            // Check bounds and if land and not visited
            if (nrow >= 0 && nrow < grid.size() &&
                ncol >= 0 && ncol < grid[0].size() &&
                !vis[nrow][ncol] && grid[nrow][ncol] == 1) {
                dfs(nrow, ncol, baseRow, baseCol, grid, vis, shape);
            }
        }
    }

    // Main function to return number of distinct islands
    int countDistinctIslands(vector<vector<int>>& grid) {
        int n = grid.size();
        int m = grid[0].size();
        vector<vector<int>> vis(n, vector<int>(m, 0));
        set<vector<pair<int, int>>> st;

        // Traverse grid
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                // If land and not visited
                if (grid[i][j] == 1 && !vis[i][j]) {
                    vector<pair<int, int>> shape;
                    dfs(i, j, i, j, grid, vis, shape);
                    st.insert(shape);
                }
            }
        }
        return st.size();
    }
};

// Driver code
int main() {
    vector<vector<int>> grid = {
        {1, 1, 0, 0},
        {1, 0, 0, 0},
        {0, 0, 1, 1},
        {0, 0, 1, 1}
    };
    Solution obj;
    cout << obj.countDistinctIslands(grid);
    return 0;
}
```

## Complexity Analysis
**Time Complexity: O(N*M)**, DFS traversal and marking visited cells dominate.  
**Space Complexity: O(N*M)**, or visited grid and set storing unique island shapes.
