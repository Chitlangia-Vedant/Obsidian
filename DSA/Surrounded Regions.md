# [130. Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)

You are given an `m x n` matrix `board` containing **letters** `'X'` and `'O'`, **capture regions** that are **surrounded**:

- **Connect**: A cell is connected to adjacent cells horizontally or vertically.
- **Region**: To form a region **connect every** `'O'` cell.
- **Surround**: A region is surrounded if none of the `'O'` cells in that region are on the edge of the board. Such regions are **completely enclosed** by `'X'` cells.

To capture a **surrounded region**, replace all `'O'`s with `'X'`s **in-place** within the original board. You do not need to return anything.

**Example 1:**

**Input:** board = `[["X","X","X","X"],["X","O","O","X"],["X","X","O","X"],["X","O","X","X"]]`

**Output:** `[["X","X","X","X"],["X","X","X","X"],["X","X","X","X"],["X","O","X","X"]]`

**Explanation:**

![](https://assets.leetcode.com/uploads/2021/02/19/xogrid.jpg)

In the above diagram, the bottom region is not captured because it is on the edge of the board and cannot be surrounded.

**Example 2:**

**Input:** board = `[["X"]]`

**Output:** `[["X"]]`

**Constraints:**

- `m == board.length`
- `n == board[i].length`
- `1 <= m, n <= 200`
- `board[i][j]` is `'X'` or `'O'`.

```cpp
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        
    }
};
```

# Solution
## Intuition  
  
An `'O'` should only remain `'O'` if it is connected to the border.  
  
Any `'O'` that is **not** connected to the border is completely surrounded by `'X'` and must be flipped.  
  
Instead of trying to identify surrounded regions directly, we do the opposite:  
  
1. Start DFS from every border `'O'`.  
2. Temporarily mark every border-connected `'O'` as `'0'` (safe).  
3. Traverse the board and flip every remaining `'O'` to `'X'` because these are surrounded.  
4. Finally, convert every temporary `'0'` back to `'O'`.  
  
This way, only the enclosed regions are captured.  
  
---  
  
## Algorithm  
  
### Step 1: Mark all border-connected regions  
  
Traverse all four borders.  
  
For every border cell containing `'O'`, perform DFS (`f`) to mark every connected `'O'` as `'0'`.  
  
```  
O O X  
O X X  
X O O  
  
↓  
  
0 0 X  
0 X X  
X O O  
```  
  
Only border-connected cells become `'0'`.  
  
---  
  
### Step 2: Capture surrounded regions  
  
Traverse the interior of the board (excluding borders).  
  
If a cell is still `'O'`, it means DFS never reached it, so it is completely surrounded.  
  
Convert it to `'X'`.  
  
```  
0 0 X  
0 X X  
X O O  
  
↓  
  
0 0 X  
0 X X  
X X X  
```  
  
---  
  
### Step 3: Restore safe cells  
  
Again traverse the borders.  
  
For every temporary `'0'`, perform DFS (`f2`) to change the entire connected component back to `'O'`.  
  
Final board:  
  
```  
O O X  
O X X  
X X X  
```  
  
---  
  
## Dry Run  
  
Input:  
  
```  
X X X X  
X O O X  
X X O X  
X O X X  
```  
  
### Border DFS  
  
The only border `'O'` is at `(3,1)`.  
  
Mark it as safe:  
  
```  
X X X X  
X O O X  
X X O X  
X 0 X X  
```  
  
---  
  
### Flip remaining `'O'`  
  
Every interior `'O'` is surrounded.  
  
```  
X X X X  
X X X X  
X X X X  
X 0 X X  
```  
  
---  
  
### Restore safe cells  
  
Convert `'0'` back to `'O'`.  
  
```  
X X X X  
X X X X  
X X X X  
X O X X  
```  
  
Answer:  
  
```  
[  
["X","X","X","X"],  
["X","X","X","X"],  
["X","X","X","X"],  
["X","O","X","X"]  
]  
```
## CODE (Mine)

```cpp
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        int i=0,j=0;
        while(i<board.size()||j<board[0].size()){
            if(j<board[0].size()) {
                if(board[0][j]=='O') f(0,j,board);
                if(board[board.size()-1][j]=='O') f(board.size()-1,j,board);
                j++;
            }
            if(i<board.size()) {
                if(board[i][0]=='O') f(i,0,board);
                if(board[i][board[0].size()-1]=='O') f(i,board[0].size()-1,board);
                i++;
            }
        }
        for(int a=1;a<board.size()-1;a++){
            for(int b=1;b<board[0].size()-1;b++){
                //cout<<board[a][b]<<" ";
                if(board[a][b]=='O'){
                    board[a][b]='X';
                }
            }
            //cout<<"\n";
        }
        i=0,j=0;
        while(i<board.size()||j<board[0].size()){
            if(j<board[0].size()) {
                if(board[0][j]=='0') f2(0,j,board);
                if(board[board.size()-1][j]=='0') f2(board.size()-1,j,board);
                j++;
            }
            if(i<board.size()) {
                if(board[i][0]=='0') f2(i,0,board);
                if(board[i][board[0].size()-1]=='0') f2(i,board[0].size()-1,board);
                i++;
            }
        }
        return ;
    }
    void f2(int i,int j,vector<vector<char>>& grid){
        grid[i][j]='O';
        if(i>0&&grid[i-1][j]=='0') f2(i-1,j,grid);
        if(i<grid.size()-1&&grid[i+1][j]=='0') f2(i+1,j,grid);
        if(j>0&&grid[i][j-1]=='0') f2(i,j-1,grid);
        if(j<grid[0].size()-1&&grid[i][j+1]=='0') f2(i,j+1,grid);
        
        return;
    }
    void f(int i,int j,vector<vector<char>>& grid){
        grid[i][j]='0';
        if(i>0&&grid[i-1][j]=='O') f(i-1,j,grid);
        if(i<grid.size()-1&&grid[i+1][j]=='O') f(i+1,j,grid);
        if(j>0&&grid[i][j-1]=='O') f(i,j-1,grid);
        if(j<grid[0].size()-1&&grid[i][j+1]=='O') f(i,j+1,grid);
        
        return;
    }
};
```