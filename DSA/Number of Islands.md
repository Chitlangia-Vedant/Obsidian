# [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return _the number of islands_.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

**Example 1:**

**Input:** grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
**Output:** 1

**Example 2:**

**Input:** grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
**Output:** 3

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`.

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        
    }
};
```

# Solution
## Intuition  
  
We iterate through every cell in the grid.  
  
- If the current cell is water ('0'), we simply continue.  
- If the current cell is land ('1'), we have discovered a **new island**.  
- We increment our island count and perform a DFS to visit every connected land cell.  
- During DFS, every visited land cell is changed from '1' to '0' so it won't be counted again.  
  
By the time DFS finishes, the entire island has been "removed" from the grid. Therefore, every future encounter with a '1' represents the start of a completely new island.  
  
---  
  
## Algorithm  
  
1. Initialize `count = 0`.  
2. Traverse every cell `(i, j)` in the grid.  
3. If `grid[i][j] == '1'`:  
	- Increment `count`.  
	- Call `solve(i, j)` to flood-fill the entire island.  
1. In `solve(i, j)`:  
	- Mark the current cell as visited by changing it to `'0'`.  
	- Recursively visit all four valid neighboring cells (up, down, left, right) that are also `'1'`.  
2. After the traversal completes, return `count`.  
  
---  
  
## Dry Run  
  
Input:  
  
```
[  
['1','1','0','0','0'],  
['1','1','0','0','0'],  
['0','0','1','0','0'],  
['0','0','0','1','1']  
]  
```
  
Start scanning:  
  
First '1' found at (0,0)  
  
DFS visits:  
  
(0,0) → (0,1) → (1,1) → (1,0)  
  
Entire first island becomes:  
  
0 0 0 0 0  
0 0 0 0 0  
0 0 1 0 0  
0 0 0 1 1  
  
count = 1  
  
Continue scanning.  
  
Next '1' at (2,2)  
  
DFS removes it.  
  
count = 2  
  
Continue scanning.  
  
Next '1' at (3,3)  
  
DFS visits:  
  
(3,3) → (3,4)  
  
count = 3  
  
Answer = 3
## CODE (Mine)

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int count=0;
        for(int i=0;i<grid.size();i++){
            for(int j=0;j<grid[0].size();j++){
                if(grid[i][j]=='1'){
                    solve(i,j,grid);
                    count++;
                }
            }
        }
        return count;
    }
    void solve(int i,int j,vector<vector<char>>& grid){
        grid[i][j]='0';
        if(i>0&&grid[i-1][j]=='1') solve(i-1,j,grid);
        if(i<grid.size()-1&&grid[i+1][j]=='1') solve(i+1,j,grid);
        if(j>0&&grid[i][j-1]=='1') solve(i,j-1,grid);
        if(j<grid[0].size()-1&&grid[i][j+1]=='1') solve(i,j+1,grid);
        
        return;
    }
};
```