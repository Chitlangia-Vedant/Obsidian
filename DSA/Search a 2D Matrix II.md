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

# Approach 1
## Intuition

Each row is sorted, and each column is sorted. Your solution uses the fact that the **first and last elements of each row** tell us whether that row can contain `target`.  
First, find the first row whose first element is `>= target`. This gives an upper boundary.  
Then find the first row whose last element is `>= target`. This gives the lower boundary.  
Only the rows between these two positions can possibly contain `target`. We then binary-search each of those rows.

## Algorithm

#### 1. Get matrix dimensions

```cpp
int m=matrix.size(),n=matrix[0].size();
```

`m` = number of rows, `n` = number of columns.

#### 2. Find the upper boundary

```cpp
auto l=lower_bound(matrix.begin(),matrix.end(),target,
    [](const auto& x,auto val){return x[0]<val;});
```

This finds the first row whose **first element is >= target**.

If that first element is exactly `target`:

```cpp
if(l!=matrix.end()&&(*l)[0]==target) return true;
```

we are done.

If `l` is the first row:

```cpp
if(l==matrix.begin()) return false;
```

then every row starts with a value greater than `target`, so `target` cannot exist.

#### 3. Find the lower boundary

```cpp
auto r=lower_bound(matrix.begin(),matrix.end(),target,
    [n](const auto& x,auto val){return x[n-1]<val;});
```

This finds the first row whose **last element is >= target**.

If there is no such row:

```cpp
if(r==matrix.end()) return false;
```

then every row ends before `target`, so it cannot exist.

If the last element is exactly `target`:

```cpp
if((*r)[n-1]==target) return true;
```

we found it.

#### 4. Search only the possible rows

```cpp
for(;r<l;r++){
    if(binary_search(r->begin(),r->end(),target)) return true;
}
```

Every row from `r` up to `l-1` can potentially contain `target`.

Since each row is sorted, `binary_search()` finds the target in `O(log n)`.

If none contain it:

```cpp
return false;
```

## CODE (Mine)
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
## Dry Run

Take:
```text
matrix =
1  4  7  11 15
2  5  8  12 19
3  6  9  16 22
10 13 14 17 24
18 21 23 26 30

target = 5
```
#### Find `l`

We compare the **first element** of each row:
```text
1 < 5
2 < 5
3 < 5
10 >= 5
```

So:
```text
l → row 4
```

#### Find `r`

We compare the **last element**:
```text
15 >= 5
```

So:
```text
r → row 1
```

Therefore, only these rows can contain `5`:

```text
row 1
row 2
row 3
```

Now binary-search them.

Row 1:
```text
[1,4,7,11,15]
```
`5` is not present.

Row 2:
```text
[2,5,8,12,19]
```
`5` is found.

Return:
```text
true
```

For `target = 20`:

- `l` points to the row starting with `21` (`row 5`)
- `r` points to the row ending with `22` (`row 3`)
- Search rows 3 and 4
- `20` isn't found
- Return `false`

## Why the Boundaries Work

For a row to contain `target`, we need:

```text
first element <= target <= last element
```

Your two `lower_bound`s find exactly the rows where these conditions can overlap.

So instead of searching all `m` rows, you eliminate rows that are definitely too small or too large.

## Time & Space Complexity

Finding `l`: `O(log m)`
Finding `r`: `O(log m)`

Binary-searching the possible rows: at most `O(m log n)`

Therefore:  
**Time: `O(log m + m log n)`**
**Space: `O(1)`**

The `lower_bound` operations use iterators and don't create additional data structures.
## Key Idea

Use the first and last values of each row to eliminate impossible rows, then binary-search only the remaining rows.

# Approach 2
## Intuition

Because rows are sorted left-to-right and columns are sorted top-to-bottom, we can start from the **top-right corner**.  
At `(row, col)`:

- If `matrix[row][col] == target`, return `true`.
- If `matrix[row][col] > target`, move **left** because everything below is even larger.
- If `matrix[row][col] < target`, move **down** because everything to the left is even smaller.  
    So every move eliminates an entire row or column. This creates a staircase path through the matrix.

## Algorithm

Start at:

```cpp
int row = 0;
int col = matrix[0].size() - 1;
```

Then:

```cpp
while(row < matrix.size() && col >= 0) {
    if(matrix[row][col] == target)
        return true;
    else if(matrix[row][col] > target)
        col--;
    else
        row++;
}
return false;
```

At every step:

- `matrix[row][col] > target` → move left.
- `matrix[row][col] < target` → move down.
- Equal → found it.

## Dry Run

```text
matrix =
1  4  7  11 15
2  5  8  12 19
3  6  9  16 22
10 13 14 17 24
18 21 23 26 30

target = 5
```

Start at top-right:

```text
15 > 5 → move left
```

```text
11 > 5 → move left
```

```text
7 > 5 → move left
```

```text
4 < 5 → move down
```

Now we're at:

```text
5 == 5
```

Return `true`.

For `target = 20`:

```text
15 < 20 → down
19 < 20 → down
22 > 20 → left
16 < 20 → down
24 > 20 → left
17 < 20 → down
30 > 20 → left
26 > 20 → left
23 > 20 → left
21 > 20 → left
18 < 20 → down
```

We eventually leave the matrix, so return `false`.

## Why This Is Optimal

There are only two possible moves: **left** or **down**.

Each left move removes one column, and each down move removes one row. Therefore, we make at most:

```text
n + m
```

moves.

This gives:  
**Time: `O(m + n)`**

**Space: `O(1)`**

This is better than your original approach:

```text
Your approach: O(log m + m log n)
Staircase:     O(m + n)
```

## CODE

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size();
        int n = matrix[0].size();

        int row = 0;
        int col = n - 1;

        while(row < m && col >= 0) {
            if(matrix[row][col] == target)
                return true;

            if(matrix[row][col] > target)
                col--;
            else
                row++;
        }

        return false;
    }
};
```

## Key Idea

`Start at top-right. Too big → left. Too small → down. Each move eliminates an entire row or column, giving O(m+n).`  