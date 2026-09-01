[240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/)

Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid2.jpg)

**Input:** matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/24/searchgrid.jpg)

**Input:** matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
**Output:** false

**Constraints:**

- `m == matrix.length`
- `n == matrix[i].length`
- `1 <= n, m <= 300`
- `-109 <= matrix[i][j] <= 109`
- All the integers in each row are **sorted** in ascending order.
- All the integers in each column are **sorted** in ascending order.
- `-109 <= target <= 109`

# CODE (Mine)
```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m=matrix.size(),n=matrix[0].size();
        auto l=lower_bound(matrix.begin(),matrix.end(),target,[](const auto& x,auto val){return x[0]<val;});
        if(l!=matrix.end()&&(*l)[0]==target) return true;
        if(l==matrix.begin()) return false;
        auto r=lower_bound(matrix.begin(),matrix.end(),target,[n](const auto& x,auto val){return x[n-1]<val;});
        if(r==matrix.end()) return false;
        if((*r)[n-1]==target) return true;
        
        for(;r<l;r++){
            if(binary_search(r->begin(),r->end(),target)) return true;
        }
        return false;
    }
};
```