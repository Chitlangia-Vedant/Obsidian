[73. Set Matrix Zeroes](https://leetcode.com/problems/set-matrix-zeroes/)

Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat1.jpg)

**Input:** matrix = `[[1,1,1],[1,0,1],[1,1,1]]`
**Output:** `[[1,0,1],[0,0,0],[1,0,1]]`

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/08/17/mat2.jpg)

**Input:** matrix = `[[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
**Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`

**Constraints:**

- `m == matrix.length`
- `n == matrix[0].length`
- `1 <= m, n <= 200`
- `-231 <= matrix[i][j] <= 231 - 1`

# Approach 1
## Intuition

Your solution uses two sets:

```cpp
set<int> row, column;
```

The idea is simple:

- If `matrix[i][j] == 0`, then the entire row `i` must become `0`.
- The entire column `j` must also become `0`.

So in the first pass, store every row and column that contains a zero.

Then in the second pass, if the current cell belongs to one of those rows or columns, set it to `0`.

## Algorithm

#### 1. Find all rows and columns containing zero

Traverse the entire matrix:

```cpp
for(int x=0;x<matrix.size();x++){
    for(int y=0;y<matrix[0].size();y++){
```

Whenever:

```cpp
matrix[x][y] == 0
```

store:

```cpp
row.insert(x);
column.insert(y);
```

So:

```text
row    = rows that must become zero
column = columns that must become zero
```

---

#### 2. Traverse the matrix again

For every cell:

```cpp
if(row.count(x) || column.count(y))
```

If its row or column contains an original zero, set:

```cpp
matrix[x][y] = 0;
```
## CODE (Mine)

```cpp
class Solution {
public:
    void setZeroes(vector<vector<int>>& matrix) {
        set<int> row,column;
        for(int x=0;x<matrix.size();x++){
            for(int y=0;y<matrix[0].size();y++){
                if(matrix[x][y]==0){
                    row.insert(x);
                    column.insert(y);
                }
            }
        }
        for(int x=0;x<matrix.size();x++){
            for(int y=0;y<matrix[0].size();y++){
                if(row.count(x) || column.count(y)){
                    matrix[x][y]=0;
                }
            }
        }    
        return;
    }
};
```
## Dry Run

Input:

```text
matrix =
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

#### First Pass

We find:

```text
matrix[1][1] = 0
```

So:

```text
row = {1}
column = {1}
```

---

#### Second Pass

Now check every cell.

Row `1` must become zero:

```text
[1,0,1] → [0,0,0]
```

Column `1` must also become zero:

```text
matrix[0][1] = 0
matrix[1][1] = 0
matrix[2][1] = 0
```

Final matrix:

```text
[
 [1,0,1],
 [0,0,0],
 [1,0,1]
]
```

## Why Two Passes Are Needed

We should not immediately turn the row and column into zero when we first encounter a zero.

For example:

```text
[
 [1,1,1],
 [1,0,1],
 [1,1,1]
]
```

If we immediately modify row `1`, we create new zeroes:

```text
[0,0,0]
```

Those new zeroes could incorrectly make other rows and columns zero.

So first we record the positions of the **original zeroes**, then modify the matrix afterward.

## Time & Space Complexity

Let:

```text
m = number of rows
n = number of columns
```

We traverse the matrix twice:

**Time:** `O(m × n)`

You use two sets:

```cpp
set<int> row;
set<int> column;
```

They can contain at most `m` rows and `n` columns.

**Space:** `O(m + n)`

Because `std::set` operations are `O(log m)` / `O(log n)`, the exact worst-case runtime with your implementation is technically:

```text
O(m × n × log(max(m,n)))
```

If you used `unordered_set` or boolean arrays instead, it would be closer to:

```text
O(m × n)
```

## Key Idea

First remember:

```text
which rows contain zero?
which columns contain zero?
```

Then zero every cell whose row or column was marked.

```text
Pass 1 → MARK rows and columns
Pass 2 → SET zeroes
```

# Approach

We will solve this question with O(1) space. We can't use extra data structure, so we use input array to keep something.

```csharp
matrix = 
[1,1,1]
[1,0,1]
[0,1,1]
```

My strategy is to use the first row and first column as a note. More precisely, if we find `0`, update the same row and column at the first row and at the first column with `0`.

Before that, we have to check if we have `0` at index `0` of row and column, I'll explain why we need this check later.

In this case, we have `0` at `[0][2]`. Let's say we have flags for row and column.

```javascript
first_row_has_zero = false
first_col_has_zero = true
```

Next we iterate through except the first row and column. In the end, we find `0` at `[1][1]`, so update `[1][0]` and `[0][1]`.

```csharp
[1,0,1]
[0,0,1]
[0,1,1]
```

Then update `row 1`, `row 2` and, `column 1`.

```csharp
[1,0,1]
[0,0,0]
[0,0,0]
```

At last, we check the flags we created in the first step. If the flags are `true`, we will update corresponding row or column. In the end,

```csharp
return [[0,0,1],[0,0,0],[0,0,0]]

[0,0,1]
[0,0,0]
[0,0,0]
```

- Why do we have the flags?

Look at the matrix before we check flags.

```csharp
[1,0,1]
[0,0,0]
[0,0,0]
```

Next step is to update the first row and the first column if needed. But **can you tell if a 0 was originally at the first row or the first column without using flags?** It's almost impossible.

```csharp
              note 0
                ↓
             [1,0,1]
    note 0 → [0,0,0]
original 0 → [0,0,0]
```

That's why we use the flags.

## Complexity

Based on Python. Other language might be different.

- Time complexity: O(m∗n)

- Space complexity: O(1)
## CODE

```python
class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        rows = len(matrix)
        cols = len(matrix[0])

        first_row_has_zero = False        
        first_col_has_zero = False

        # check if the first row contains zero
        for c in range(cols):
            if matrix[0][c] == 0:
                first_row_has_zero = True
                break

        # check if the first column contains zero
        for r in range(rows):
            if matrix[r][0] == 0:
                first_col_has_zero = True
                break
        
        # use the first row and column as a note
        for r in range(1, rows):
            for c in range(1, cols):
                if matrix[r][c] == 0:
                    matrix[r][0] = 0
                    matrix[0][c] = 0
        
        # set the marked rows to zero
        for r in range(1, rows):
            if matrix[r][0] == 0:
                for c in range(1, cols):
                    matrix[r][c] = 0

        # set the marked columns to zero
        for c in range(1, cols):
            if matrix[0][c] == 0:
                for r in range(1, rows):
                    matrix[r][c] = 0
    
        # set the first row to zero if needed
        if first_row_has_zero:
            for c in range(cols):
                matrix[0][c] = 0

        # set the first column to zero if needed
        if first_col_has_zero:
            for r in range(rows):
                matrix[r][0] = 0
        
        return matrix
```