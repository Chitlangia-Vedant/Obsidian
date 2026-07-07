# [304. Range Sum Query 2D - Immutable](https://leetcode.com/problems/range-sum-query-2d-immutable/)

Given a 2D matrix `matrix`, handle multiple queries of the following type:

- Calculate the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

Implement the `NumMatrix` class:

- `NumMatrix(int[][] matrix)` Initializes the object with the integer matrix `matrix`.
- `int sumRegion(int row1, int col1, int row2, int col2)` Returns the **sum** of the elements of `matrix` inside the rectangle defined by its **upper left corner** `(row1, col1)` and **lower right corner** `(row2, col2)`.

You must design an algorithm where `sumRegion` works on `O(1)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/14/sum-grid.jpg)


**Input**
```
["NumMatrix", "sumRegion", "sumRegion", "sumRegion"]
[[[[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]], [2, 1, 4, 3], [1, 1, 2, 2], [1, 2, 2, 4]]
```
**Output**
```
[null, 8, 11, 12]
```


**Explanation**
```
NumMatrix numMatrix = new NumMatrix([[3, 0, 1, 4, 2], [5, 6, 3, 2, 1], [1, 2, 0, 1, 5], [4, 1, 0, 1, 7], [1, 0, 3, 0, 5]]);
```
numMatrix.sumRegion(2, 1, 4, 3); // return 8 (i.e sum of the red rectangle)
numMatrix.sumRegion(1, 1, 2, 2); // return 11 (i.e sum of the green rectangle)
numMatrix.sumRegion(1, 2, 2, 4); // return 12 (i.e sum of the blue rectangle)

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= m, n <= 200`
- `-104 <= matrix[i][j] <= 104`
- `0 <= row1 <= row2 < m`
- `0 <= col1 <= col2 < n`
- At most `104` calls will be made to `sumRegion`.

```cpp
class NumMatrix {
public:
    NumMatrix(vector<vector<int>>& matrix) {
        
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        
    }
};
```

# Solution 1 (TLE)

```cpp
class NumMatrix {
private:
    vector<vector<int>> mat;
public:
    NumMatrix(vector<vector<int>>& matrix): mat{matrix}{
        
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        int sum=0;
        for(int i=row1;i<=row2;i++)
            sum=accumulate(mat[i].begin()+col1,mat[i].begin()+col2+1,sum);
        return sum;
    }
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix* obj = new NumMatrix(matrix);
 * int param_1 = obj->sumRegion(row1,col1,row2,col2);
 */
```

# Solution 2

Construct a 2D array `sums[row+1][col+1]`

(**notice**: we add additional blank row `sums[0][col+1]={0}` and blank column `sums[row+1][0]={0}` to remove the edge case checking), so, we can have the following definition

`sums[i+1][j+1]` represents the sum of area from `matrix[0][0]` to `matrix[i][j]`

To calculate sums, the ideas as below

```sql
+-----+-+-------+     +--------+-----+     +-----+---------+     +-----+--------+
|     | |       |     |        |     |     |     |         |     |     |        |
|     | |       |     |        |     |     |     |         |     |     |        |
+-----+-+       |     +--------+     |     |     |         |     +-----+        |
|     | |       |  =  |              |  +  |     |         |  -  |              |
+-----+-+       |     |              |     +-----+         |     |              |
|               |     |              |     |               |     |              |
|               |     |              |     |               |     |              |
+---------------+     +--------------+     +---------------+     +--------------+

   sums[i][j]      =    sums[i-1][j]    +     sums[i][j-1]    -   sums[i-1][j-1]                    +   matrix[i-1][j-1]
```

So, we use the same idea to find the specific area's sum.

```sql
+---------------+   +---------+----+   +---+-----------+   +---------+----+
|               |   |         |    |   |   |           |   |         |    |
|   (r1,c1)     |   |         |    |   |   |           |   |         |    |    
|   +------+    |   |         |    |   |   |           |   +---------+    |      
|   |      |    | = |         |    | - |   |           | - |      (r1,c2) |   
|   |      |    |   |         |    |   |   |           |   |              |
|   +------+    |   +---------+    |   +---+           |   |              |
|        (r2,c2)|   |       (r2,c2)|   |   (r2,c1)     |   |              |
+---------------+   +--------------+   +---------------+   +--------------+   

				    +---+----------+
				    |   |          |
				    |   |          |
				    +---+          |
				  + |    (r1,c1)   |
				    |              |
				    |              |
				    |              |
				    +--------------+
```

##

```cpp
class NumMatrix {
private:
    int row, col;
    vector<vector<int>> sums;
public:
    NumMatrix(vector<vector<int>> &matrix) {
        row = matrix.size();
        col = row>0 ? matrix[0].size() : 0;
        sums = vector<vector<int>>(row+1, vector<int>(col+1, 0));
        for(int i=1; i<=row; i++) {
            for(int j=1; j<=col; j++) {
                sums[i][j] = matrix[i-1][j-1] + 
                             sums[i-1][j] + sums[i][j-1] - sums[i-1][j-1] ;
            }
        }
    }

    int sumRegion(int row1, int col1, int row2, int col2) {
        return sums[row2+1][col2+1] - sums[row2+1][col1] - sums[row1][col2+1] + sums[row1][col1];
    }
};
```