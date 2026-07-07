# [1351. Count Negative Numbers in a Sorted Matrix](https://leetcode.com/problems/count-negative-numbers-in-a-sorted-matrix/)

Given a `m x n` matrix `grid` which is sorted in non-increasing order both row-wise and column-wise, return _the number of **negative** numbers in_ `grid`.

**Example 1:**

**Input:** grid = [[4,3,2,-1],[3,2,1,-1],[1,1,-1,-2],[-1,-1,-2,-3]]
**Output:** 8
**Explanation:** There are 8 negatives number in the matrix.

**Example 2:**

**Input:** grid = [[3,2],[1,0]]
**Output:** 0

**Constraints:**

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 100`
- `-100 <= grid[i][j] <= 100`

**Follow up:** Could you find an `O(n + m)` solution?

```cpp
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        
    }
};
```

# Solution 1

## **Intuition**

- So from what we can discern from the given inputs that the rows will be non-increasing which means that row[index] > row[index+1].
- Column[index] > Column[index+1].
- To count the number of negative numbers we have to keep count while parsing through the matrix.

## **Main Solution :**

- The approach is simple. We take one element and compare it to zero and if it's lower than zero (a negative number) then we increment the counter otherwise we move ahead.
- The number to be taken is in this case would be the right top most because it will be the lowest for both rows and columns. Thus, easier to compare.
- We calculate the number of rows and columns present and initiate a counter and two variables for row from first row and column from last column.  

![[Pasted image 20260615123218.png]]

- Now, we parse through the matrix with the breaking conditions of the while loop being the row counter to be less than row length and column counter to be bigger than or equal to zero.
- We take the element and compare it to zero, now as we are on the first row and last column. The element would be the biggest in it's column and smallest in it's row.
- So, the main logic is if the selected element is lower than zero then for sure, all the elements in the column below it would be lower than zero because that element would be the biggest. We increment the counter by all columns and then reduce the column pointer by 1.
- Now, if the selected element is bigger than zero then it would mean all elements on the left side of it in it's row would be bigger than zero. So, we just increment row by 1 and ignore the whole row. Counter will remain the same.
- We will parse through the array checking these conditions and then return final count.

Here's the rest of visual explanation

![[Pasted image 20260615123241.png]]
![[Pasted image 20260615123254.png]]
![[Pasted image 20260615123307.png]]
![[Pasted image 20260615123326.png]]
![[Pasted image 20260615123352.png]]
![[Pasted image 20260615123452.png]]
![[Pasted image 20260615123427.png]]
![[Pasted image 20260615123411.png]]

## CODE (Mine)
```cpp
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m=grid.size(),n=grid[0].size();
        int j=n;
        int count=0;
        for(int i=0;i<m;i++){
            while(grid[i][j-1]<0){
                j--;
                if(j==0){
                    count+=(m-i)*n;
                    return count;
                }
                    
            }
            count+=n-j;
        }
        return count;
    }
};
```

