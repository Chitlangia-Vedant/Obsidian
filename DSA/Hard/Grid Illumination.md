# [1001. Grid Illumination](https://leetcode.com/problems/grid-illumination/)

There is a 2D `grid` of size `n x n` where each cell of this grid has a lamp that is initially **turned off**.

You are given a 2D array of lamp positions `lamps`, where `lamps[i] = [rowi, coli]` indicates that the lamp at `grid[rowi][coli]` is **turned on**. Even if the same lamp is listed more than once, it is turned on.

When a lamp is turned on, it **illuminates its cell** and **all other cells** in the same **row, column, or diagonal**.

You are also given another 2D array `queries`, where `queries[j] = [rowj, colj]`. For the `jth` query, determine whether `grid[rowj][colj]` is illuminated or not. After answering the `jth` query, **turn off** the lamp at `grid[rowj][colj]` and its **8 adjacent lamps** if they exist. A lamp is adjacent if its cell shares either a side or corner with `grid[rowj][colj]`.

Return _an array of integers_ `ans`_,_ _where_ `ans[j]` _should be_ `1` _if the cell in the_ `jth` _query was illuminated, or_ `0` _if the lamp was not._

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/08/19/illu_1.jpg)

**Input:** n = 5, lamps = `[[0,0],[4,4]]`, queries = `[[1,1],[1,0]]`
**Output:** [1,0]
**Explanation:** We have the initial grid with all lamps turned off. In the above picture we see the grid after turning on the lamp at grid[0][0] then turning on the lamp at grid[4][4].
The 0th query asks if the lamp at grid[1][1] is illuminated or not (the blue square). It is illuminated, so set ans[0] = 1. Then, we turn off all lamps in the red square.
![](https://assets.leetcode.com/uploads/2020/08/19/illu_step1.jpg)
The 1st query asks if the lamp at grid[1][0] is illuminated or not (the blue square). It is not illuminated, so set ans[1] = 0. Then, we turn off all lamps in the red rectangle.
![](https://assets.leetcode.com/uploads/2020/08/19/illu_step2.jpg)

**Example 2:**

**Input:** n = 5, lamps = `[[0,0],[4,4]], `queries = `[[1,1],[1,1]]`
**Output:** [1,1]

**Example 3:**

**Input:** n = 5, lamps = `[[0,0],[0,4]]`, queries = `[[0,4],[0,1],[1,4]]`
**Output:** [1,1,0]

**Constraints:**

- `1 <= n <= 109`
- `0 <= lamps.length <= 20000`
- `0 <= queries.length <= 20000`
- `lamps[i].length == 2`
- `0 <= rowi, coli < n`
- `queries[j].length == 2`
- `0 <= rowj, colj < n`

```cpp
class Solution {
public:
    vector<int> gridIllumination(int n, vector<vector<int>>& lamps, vector<vector<int>>& queries) {
        
    }
};
```

# Solution

## Intuition

A lamp illuminates:

- its entire row,
- its entire column,
- its main diagonal (`row - col`),
- its anti-diagonal (`row + col`).

Instead of checking every lamp for every query (which would be too slow), we keep track of **how many active lamps** illuminate each row, column, and diagonal.

For every query:

1. Check whether its row, column, or either diagonal has at least one active lamp.
2. Record the answer.
3. Turn off any lamp located at the query cell or one of its 8 neighboring cells.

This allows every query to be answered in nearly constant time.

---

## Data Structures

### `x_axis`

```
row -> number of active lamps
```

Example:

```
Row 3 has 4 lamps

x_axis[3] = 4
```

---

### `y_axis`

```
column -> number of active lamps
```

---

### `sub`

Stores counts for the **main diagonals**.

Every cell on the same main diagonal has the same value of:

```
row - column
```

Example:

```
(0,0)
(1,1)
(2,2)
```

All have

```
row - column = 0
```

---

### `add`

Stores counts for the **anti-diagonals**.

Every cell on the same anti-diagonal has the same value of:

```
row + column
```

Example:

```
(0,3)
(1,2)
(2,1)
(3,0)
```

All have

```
row + column = 3
```

---

### `lamap`

```
map<vector<int>, int>
```

Stores whether a lamp currently exists at a position.

This lets us quickly determine whether a lamp should be turned off after each query.

Duplicate lamps are ignored during initialization.

---

### `dir`

```
(0,0)
```

plus the eight neighboring directions.

These are exactly the cells whose lamps must be switched off after each query.

---

# Algorithm

## Step 1: Initialize

For every lamp:

- Ignore duplicates.
- Increase the count for:
  - its row,
  - its column,
  - its main diagonal,
  - its anti-diagonal.
- Store the lamp in `lamap`.

---

## Step 2: Process Each Query

For a query `(r,c)`:

The cell is illuminated if **any** of these are non-zero:

```cpp
x_axis[r]
y_axis[c]
sub[r-c]
add[r+c]
```

If any count is positive:

```
answer = 1
```

otherwise

```
answer = 0
```

---

## Step 3: Turn Off Lamps

For each of the 9 surrounding cells:

```
(r,c)

(r-1,c-1)
(r-1,c)
(r-1,c+1)

(r,c-1)
(r,c+1)

(r+1,c-1)
(r+1,c)
(r+1,c+1)
```

If a lamp exists there:

- Remove it.
- Decrease all four counters.

This ensures future queries use only the remaining active lamps.

---

## Dry Run

Suppose

```
Lamps:

(0,0)
(4,4)
```

---

### Initialization

```
Rows

0 -> 1
4 -> 1

Columns

0 -> 1
4 -> 1

Main diagonals

0 -> 2

Anti diagonals

0 -> 1
8 -> 1
```

---

### Query

```
(1,1)
```

Check

```
row 1 = 0
column 1 = 0
main diagonal 0 = 2
anti diagonal 2 = 0
```

Main diagonal contains active lamps.

Answer:

```
1
```

Now remove lamps around `(1,1)`.

Only `(0,0)` exists.

Remove it.

Updated counts:

```
Row 0 -> 0
Column 0 -> 0
Main diagonal 0 -> 1
Anti diagonal 0 -> 0
```

---

### Next Query

Future illumination is computed using the updated counts, reflecting that `(0,0)` is no longer active.

---
## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> res;
    vector<pair<int, int>> dir = {{0,0},{1,1},{-1,-1},{1,-1},{-1,1},{-1,0}, {1,0}, {0,-1}, {0,1}};
    vector<int> gridIllumination(int n, vector<vector<int>>& lamps, vector<vector<int>>& queries) {
        unordered_map<int,int> x_axis,y_axis,add,sub;
        map<vector<int>,int> lamap;
        for(auto &y:lamps){
            if(!lamap.contains(y))
            {
                x_axis[y[0]]++;
                y_axis[y[1]]++;
                sub[y[0]-y[1]]++;
                add[y[0]+y[1]]++;
                lamap[y]++;
            }
        }
        for(auto &x:queries){
            if(x_axis[x[0]]>0||y_axis[x[1]]>0||sub[x[0]-x[1]]>0||add[x[0]+x[1]]>0)
                res.push_back(1);
            else
                res.push_back(0);
            for (auto [dx, dy] : dir) {
                if(lamap[{x[0]+dx,x[1]+dy}]>0){
                    lamap[{x[0]+dx,x[1]+dy}]--;
                    x_axis[x[0]+dx]--;
                    y_axis[x[1]+dy]--;
                    sub[(x[0]+dx)-(x[1]+dy)]--;
                    add[(x[0]+dx)+(x[1]+dy)]--;
                }
            }
        }
        return res;
    }
};
```