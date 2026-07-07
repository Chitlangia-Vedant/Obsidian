#   [51. N-Queens](https://leetcode.com/problems/n-queens/)

The **n-queens** puzzle is the problem of placing `n` queens on an `n x n` chessboard such that no two queens attack each other.

Given an integer `n`, return _all distinct solutions to the **n-queens puzzle**_. You may return the answer in **any order**.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` both indicate a queen and an empty space, respectively.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/queens.jpg)

**Input:** n = 4
**Output:** `[[".Q..","...Q","Q...","..Q."],["..Q.","Q...","...Q",".Q.."]]`
**Explanation:** There exist two distinct solutions to the 4-queens puzzle as shown above

**Example 2:**

**Input:** n = 1
**Output:** `[["Q"]]`

**Constraints:**

- `1 <= n <= 9`

# Solution
## Intuition  
  
Since each queen attacks its entire row, we place **exactly one queen per row**.  
  
We build the board row by row using **backtracking**.  
  
For the current row, we try placing a queen in every column.  
  
A position is valid only if it does **not** share:  
- the same column,  
- the main diagonal (`row - col`),  
- the anti-diagonal (`row + col`)  
  
with any previously placed queen.  
  
If the position is valid, we place the queen and recursively solve the next row.  
  
If no solution is possible from that choice, we backtrack and try another column.  
  
---  
  
## Representation  
  
Instead of storing the entire chessboard during recursion, we only store the column position of the queen in each row.  
  
```  
queens = [1, 3, 0, 2]  
```  
  
means  
  
```  
Row 0 → Column 1  
Row 1 → Column 3  
Row 2 → Column 0  
Row 3 → Column 2  
```  
  
which corresponds to  
  
```  
.Q..  
...Q  
Q...  
..Q.  
```  
  
This representation is compact and makes conflict checking simple.  
  
---  
  
## Algorithm  
  
1. Start with an empty `queens` vector.  
2. Let `x = queens.size()`, which represents the current row.  
3. For every column in the current row:  
- Check whether placing a queen there conflicts with any previous queen.  
4. A conflict exists if:  
- Same column:  
```  
col == queens[previousRow]  
```  
- Same main diagonal:  
```  
row - col == previousRow - previousCol  
```  
Implemented as:  
```cpp  
x - i == j - queens[j]  
```  
- Same anti-diagonal:  
```  
row + col == previousRow + previousCol  
```  
Implemented as:  
```cpp  
x + i == j + queens[j]  
```  
5. If the position is valid:  
- Add the column to `queens`.  
- Recurse for the next row.  
6. When `queens.size() == n`, every row has a queen, so construct the board and add it to the answer.  
  
---  
  
## Dry Run (n = 4)  
  
Start:  
  
```  
queens = []  
```  
  
### Row 0  
  
Try every column.  
  
Choose column 1.  
  
```  
queens = [1]  
```  
  
Board:  
  
```  
.Q..  
....  
....  
....  
```  
  
---  
  
### Row 1  
  
Columns:  
  
```  
0 ❌ diagonal  
1 ❌ same column  
2 ❌ diagonal  
3 ✅  
```  
  
```  
queens = [1,3]  
```  
  
Board:  
  
```  
.Q..  
...Q  
....  
....  
```  
  
---  
  
### Row 2  
  
Columns:  
  
```  
0 ✅  
1 ❌ same column  
2 ❌ diagonal  
3 ❌ same column  
```  
  
```  
queens = [1,3,0]  
```  
  
Board:  
  
```  
.Q..  
...Q  
Q...  
....  
```  
  
---  
  
### Row 3  
  
Columns:  
  
```  
0 ❌ same column  
1 ❌ same column  
2 ✅  
3 ❌ same column  
```  
  
```  
queens = [1,3,0,2]  
```  
  
All 4 queens are placed.  
  
Construct the board:  
  
```  
.Q..  
...Q  
Q...  
..Q.  
```  
  
Store this solution.  
  
Backtracking continues to discover the remaining valid arrangement.
## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<string>> ans;
    vector<vector<string>> solveNQueens(int n) {
       solve(n,{});
       return ans;
    }
    void solve(int n,vector<int> queens){
        int x=queens.size();
        if(x==n){
            vector<string> grid;
            for(int i=0;i<n;i++){
                string row="";
                for(int j=0;j<n;j++){
                    
                    row+=j==queens[i]?'Q':'.';
                }
                grid.push_back(row);
            }
            ans.push_back(grid);
            return;
        }
        for(int i=0;i<n;i++){
            bool b=true;
            for(int j=0;j<x;j++){
                if(i==queens[j]||x-i==j-queens[j]||x+i==j+queens[j]){
                    b=false;
                    break;
                }
            }
            if(b){
                vector<int> temp=queens;
                temp.push_back(i);
                solve(n,temp);
            }
        }
        return;
    }
};
```