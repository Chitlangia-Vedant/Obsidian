# 434 · Number of Islands II

Given a n,m which means the row and column of the 2D matrix and an array of pair A( size k). Originally, the 2D matrix is all 0 which means there is only sea in the matrix. The list pair has k operator and each operator has two integer A[i].x, A[i].y means that you can change the grid matrix[A[i].x][A[i].y] from sea to island. Return how many island are there in the matrix after each operator. You need to return an array of size K.

0 is represented as the sea, 1 is represented as the island. If two 1 is adjacent, we consider them in the same island. We only consider up/down/left/right adjacent.

Example

**Example 1:**

```
Input: n = 4, m = 5, A = [[1,1],[0,1],[3,3],[3,4]]
Output: [1,1,2,2]
Explanation:
0.  00000
    00000
    00000
    00000
1.  00000
    01000
    00000
    00000
2.  01000
    01000
    00000
    00000
3.  01000
    01000
    00000
    00010
4.  01000
    01000
    00000
    00011
```

**Example 2:**

```
Input: n = 3, m = 3, A = [[0,0],[0,1],[2,2],[2,1]]
Output: [1,1,2,2]
```

```cpp
/**
 * Definition for a point.
 * struct Point {
 *     int x;
 *     int y;
 *     Point() : x(0), y(0) {}
 *     Point(int a, int b) : x(a), y(b) {}
 * };
 */

class Solution {
public:
    /**
     * @param n: An integer
     * @param m: An integer
     * @param operators: an array of point
     * @return: an integer array
     */

};
```

# Solution
The solution uses a Union-Find data structure to efficiently track and merge islands. Let's walk through the implementation step by step:

**1. Union-Find Data Structure Setup**

We create a `UnionFind` class that maintains:

- `p`: parent array where `p[x]` stores the parent of node `x`
- `size`: array tracking the size of each component
- `find(x)`: finds the root of the component containing `x`, with path compression for optimization
- `union(a, b)`: merges two components and returns `True` if they were previously separate, `False` if already connected

**2. Grid Initialization**

- Create a 2D `grid` of size `m x n`, initially all zeros (water)
- Initialize `cnt = 0` to track the current number of islands
- Create a Union-Find instance with `m * n` nodes (one for each grid cell)

**3. Position Mapping**

Each grid position `(i, j)` is mapped to a unique index using the formula `i * n + j`. This allows us to use a 1D Union-Find structure for a 2D grid.

**4. Processing Each Position**

For each position `(i, j)` in `positions`:

- **Check if already land**: If `grid[i][j] == 1`, this position is already land, so append current `cnt` to answer and continue
    
- **Add new land**:
    
    - Set `grid[i][j] = 1`
    - Increment `cnt` by 1 (initially assume it's a new island)
- **Check adjacent cells**: Use `dirs = (-1, 0, 1, 0, -1)` with `pairwise` to generate the four directions:
    
    - Calculate adjacent position: `(x, y) = (i + a, j + b)`
    - If the adjacent cell is:
        - Within bounds: `0 <= x < m` and `0 <= y < n`
        - Is land: `grid[x][y] == 1`
        - Not yet connected: `uf.union(i * n + j, x * n + y)` returns `True`
    - Then decrement `cnt` by 1 (two islands merge into one)
- **Record result**: Append current `cnt` to the answer array
    

**5. Union Operation Details**

The `union` operation in the Union-Find:

- Takes indices adjusted by subtracting 1 (to handle 0-based indexing)
- Finds roots of both components using `find`
- If they share the same root, returns `False` (already connected)
- Otherwise, merges the smaller component into the larger one (union by size optimization)
- Returns `True` to indicate a successful merge

The time complexity is `O(k × α(m × n))` where `k` is the number of positions and `α` is the inverse Ackermann function (practically constant). The space complexity is `O(m × n)` for the grid and Union-Find structure.

## Example Walkthrough

Let's walk through a small example with a 3×3 grid and positions = `[[0,0], [0,1], [1,1], [2,1], [1,0]]`.

**Initial State:**

```
Grid:        Islands: 0
. . .
. . .
. . .
```

**Operation 1: Add land at [0,0]**

- `Grid[0][0]` = 0 (water), so we add land
- Increment island count: 0 → 1
- Check 4 neighbors: all are water
- No unions needed
- Result: 1 island

```
Grid:        Islands: 1
1 . .
. . .
. . .
```

**Operation 2: Add land at [0,1]**

- `Grid[0][1]` = 0 (water), so we add land
- Increment island count: 1 → 2
- Check neighbors:
    - Left [0,0] is land! Union(1, 0) returns True
    - Decrement count: 2 → 1
- Result: 1 island (merged with existing)

```
Grid:        Islands: 1
1 1 .
. . .
. . .
```

**Operation 3: Add land at [1,1]**

- `Grid[1][1]` = 0 (water), so we add land
- Increment island count: 1 → 2
- Check neighbors:
    - Up [0,1] is land! Union(4, 1) returns True
    - Decrement count: 2 → 1
- Result: 1 island (extended the existing island)

```
Grid:        Islands: 1
1 1 .
. 1 .
. . .
```

**Operation 4: Add land at [2,1]**

- `Grid[2][1]` = 0 (water), so we add land
- Increment island count: 1 → 2
- Check neighbors:
    - `Up [1,1] `is land! Union(7, 4) returns True
    - Decrement count: 2 → 1
- Result: 1 island (still one connected island)

```
Grid:        Islands: 1
1 1 .
. 1 .
. 1 .
```

**Operation 5: Add land at [1,0]**

- `Grid[1][0]` = 0 (water), so we add land
- Increment island count: 1 → 2
- Check neighbors:
    - Up [0,0] is land! Union(3, 0) returns True
    - Decrement count: 2 → 1
    - Right [1,1] is land! Union(3, 4) returns False (already connected through [0,0]→[0,1]→[1,1])
    - No decrement since they're already in the same component
- Result: 1 island

```
Grid:        Islands: 1
1 1 .
1 1 .
. 1 .
```

**Final Answer: [1, 1, 1, 1, 1]**

The key insight is how Union-Find efficiently tracks connectivity. When we try to union [1,0] with [1,1] in the last step, they're already connected through the path [1,0]→[0,0]→[0,1]→[1,1], so union returns False and we don't double-count the merge.

## CODE

```cpp
class UnionFind {
public:
    // Initialize Union-Find data structure with n elements
    UnionFind(int n) {
        parent = vector<int>(n);
        componentSize = vector<int>(n, 1);
        // Initially, each element is its own parent (self-loop)
        iota(parent.begin(), parent.end(), 0);
    }

    // Unite two components containing elements a and b
    // Returns true if they were in different components, false otherwise
    bool unite(int a, int b) {
        int rootA = find(a);
        int rootB = find(b);

        // Already in the same component
        if (rootA == rootB) {
            return false;
        }

        // Union by size: attach smaller tree under root of larger tree
        if (componentSize[rootA] > componentSize[rootB]) {
            parent[rootB] = rootA;
            componentSize[rootA] += componentSize[rootB];
        } else {
            parent[rootA] = rootB;
            componentSize[rootB] += componentSize[rootA];
        }

        return true;
    }

    // Find the root of the component containing element x
    // Uses path compression for optimization
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);  // Path compression
        }
        return parent[x];
    }

private:
    vector<int> parent;        // Parent array for Union-Find
    vector<int> componentSize; // Size of each component for union by size
};

class Solution {
public:
    vector<int> numIslands2(int m, int n, vector<vector<int>>& positions) {
        // Initialize grid to track land cells
        int grid[m][n];
        memset(grid, 0, sizeof(grid));

        // Create Union-Find structure for all possible cells
        UnionFind unionFind(m * n);

        // Direction vectors for exploring 4 adjacent cells (up, right, down, left)
        int directions[5] = {-1, 0, 1, 0, -1};

        // Counter for number of islands
        int islandCount = 0;

        // Result vector to store island count after each operation
        vector<int> result;

        // Process each position to add land
        for (auto& position : positions) {
            int row = position[0];
            int col = position[1];

            // If this cell is already land, no change in island count
            if (grid[row][col]) {
                result.push_back(islandCount);
                continue;
            }

            // Mark this cell as land
            grid[row][col] = 1;

            // Initially, this creates a new island
            ++islandCount;

            // Check all 4 adjacent cells
            for (int k = 0; k < 4; ++k) {
                int newRow = row + directions[k];
                int newCol = col + directions[k + 1];

                // Check if adjacent cell is valid and is land
                if (newRow >= 0 && newRow < m && newCol >= 0 && newCol < n && grid[newRow][newCol]) {
                    // Convert 2D coordinates to 1D for Union-Find
                    int currentCell = row * n + col;
                    int adjacentCell = newRow * n + newCol;

                    // If unite returns true, two separate islands were merged
                    if (unionFind.unite(currentCell, adjacentCell)) {
                        --islandCount;
                    }
                }
            }

            // Store the current island count
            result.push_back(islandCount);
        }

        return result;
    }
};

```

## Time and Space Complexity

**Time Complexity:** `O(k × α(m × n))` where `k` is the length of `positions` and `α` is the inverse Ackermann function.

- For each position in `positions` (k iterations total):
    - Checking if the cell is already land: `O(1)`
    - Adding the land and incrementing count: `O(1)`
    - Checking up to 4 adjacent cells and performing union operations if needed
    - Each union operation involves two `find` operations and the actual union
    - With path compression and union by size, `find` operations take `O(α(m × n))` amortized time
    - The `union` operation itself (after finding roots) takes `O(1)`
    - Since we check at most 4 neighbors, this is `O(4 × α(m × n))` = `O(α(m × n))`

The inverse Ackermann function `α(m × n)` grows extremely slowly and can be considered effectively constant for all practical values of `m × n` (it's less than 5 for any reasonable input size).

**Space Complexity:** `O(m × n)`

- The UnionFind data structure maintains:
    - Parent array `p`: `O(m × n)`
    - Size array `size`: `O(m × n)`
- The `grid` matrix to track land cells: `O(m × n)`
- The `ans` list to store results: `O(k)`
- Recursive call stack for `find` operation: `O(log(m × n))` in worst case before path compression, but effectively `O(α(m × n))` with path compression