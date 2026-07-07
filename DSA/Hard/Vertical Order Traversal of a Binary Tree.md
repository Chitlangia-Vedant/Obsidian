# [987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)

Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(row, col)`, its left and right children will be at positions `(row + 1, col - 1)` and `(row + 1, col + 1)` respectively. The root of the tree is at `(0, 0)`.

The **vertical order traversal** of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return _the **vertical order traversal** of the binary tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** `[[9],[3,15],[20],[7]]`
**Explanation:**
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree2.jpg)

**Input:** root = [1,2,3,4,5,6,7]
**Output:** `[[4],[2],[1,5,6],[3],[7]]`
**Explanation:**
Column -2: Only node 4 is in this column.
Column -1: Only node 2 is in this column.
Column 0: Nodes 1, 5, and 6 are in this column.
          1 is at the top, so it comes first.
          5 and 6 are at the same position (2, 0), so we order them by their value, 5 before 6.
Column 1: Only node 3 is in this column.
Column 2: Only node 7 is in this column.

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/01/29/vtree3.jpg)

**Input:** root = [1,2,3,4,6,5,7]
**Output:** `[[4],[2],[1,5,6],[3],[7]]`
**Explanation:**
This case is the exact same as example 2, but with nodes 5 and 6 swapped.
Note that the solution remains the same since 5 and 6 are in the same location and should be ordered by their values.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `0 <= Node.val <= 1000`

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        
    }
};
```

# Solution 1
## Intuition

The key insight is that we need to track two pieces of information for each node: its position in 2D space and its value. Since we're asked for a vertical order traversal (column by column), we need to know which column each node belongs to.

Think of the [tree](https://algo.monster/problems/tree_intro) as being placed on a 2D grid. The root sits at position `(0, 0)`. As we move down the tree, going left means moving one column to the left (`col - 1`), and going right means moving one column to the right (`col + 1`). The row always increases by 1 as we go deeper.

The natural approach is to traverse the entire [tree](https://algo.monster/problems/tree_intro) and record where each node sits. DFS works perfectly here because:

1. We can easily pass the current position `(row, col)` as parameters during recursion
2. When visiting a left child, we update to `(row + 1, col - 1)`
3. When visiting a right child, we update to `(row + 1, col + 1)`

Once we've collected all nodes with their positions, the problem becomes a [sorting](https://algo.monster/problems/sorting_summary) problem. We need to group nodes by column, and within each column, order them by row (top to bottom). The tricky part is when multiple nodes share the same exact position - in this case, we sort by their values.

The clever trick in the solution is to store each node as a tuple `(column, row, value)`. When we sort this list, Python naturally sorts by the first element (column), then by the second element (row) for ties, and finally by the third element (value) for nodes at the same position. This gives us exactly the ordering we need.

After [sorting](https://algo.monster/problems/sorting_summary), we simply iterate through the sorted list and group consecutive nodes with the same column value into the same sublist. This produces our final vertical order traversal.
### Solution Approach

The solution uses DFS combined with [sorting](https://algo.monster/problems/sorting_summary) to achieve the vertical order traversal. Let's break down the implementation:

**1. DFS Traversal with Position Tracking**

We define a recursive function `dfs(root, i, j)` where:

- `root` is the current node
- `i` represents the row (depth level)
- `j` represents the column

During the DFS traversal:

- For each non-null node, we record its position and value as a tuple `(j, i, root.val)` in the `nodes` list
- Note the order: column first, then row, then value - this is intentional for [sorting](https://algo.monster/problems/sorting_summary) later
- Recursively visit the left child with position `(i + 1, j - 1)`
- Recursively visit the right child with position `(i + 1, j + 1)`

**2. [Sorting](https://algo.monster/problems/sorting_summary) the Collected Nodes**

After collecting all nodes with their positions, we call `nodes.sort()`. Python's default tuple [sorting](https://algo.monster/problems/sorting_summary) behavior works perfectly here:

- First sorts by column `j` (leftmost to rightmost)
- For nodes in the same column, sorts by row `i` (top to bottom)
- For nodes at the exact same position, sorts by value

**3. Grouping by Column**

We iterate through the sorted `nodes` list and group nodes by column:

- Initialize `prev = -2000` (a value outside possible column range)
- For each node `(j, _, val)` in the sorted list:
    - If the column `j` differs from `prev`, start a new group by appending an empty list to `ans`
    - Update `prev = j` to track the current column
    - Append the node's value to the last group in `ans`

The algorithm has a time complexity of `O(n log n)` where `n` is the number of nodes, dominated by the [sorting](https://algo.monster/problems/sorting_summary) step. The space complexity is `O(n)` for storing the nodes list.

This approach elegantly handles all the requirements: vertical ordering by column, top-to-bottom ordering within columns, and value-based [sorting](https://algo.monster/problems/sorting_summary) for nodes at the same position
## Example Walkthrough

Let's walk through a small example to illustrate the solution approach.

Consider this binary tree:

```
       3
      / \
     9   20
        /  \
       15   7
```

**Step 1: DFS Traversal with Position Tracking**

Starting from the root at position (0, 0), we traverse the tree and record each node's position:

- Visit node 3 at (row=0, col=0) → Record: (0, 0, 3)
- Visit left child 9 at (row=1, col=-1) → Record: (-1, 1, 9)
- Visit right child 20 at (row=1, col=1) → Record: (1, 1, 20)
- Visit 20's left child 15 at (row=2, col=0) → Record: (0, 2, 15)
- Visit 20's right child 7 at (row=2, col=2) → Record: (2, 2, 7)

Our `nodes` list now contains:

```
[(0, 0, 3), (-1, 1, 9), (1, 1, 20), (0, 2, 15), (2, 2, 7)]
```

**Step 2: Sorting**

Sort the list by (column, row, value):

```
[(-1, 1, 9), (0, 0, 3), (0, 2, 15), (1, 1, 20), (2, 2, 7)]
```

Notice how the sorting groups nodes by column first:

- Column -1: [(9)]
- Column 0: [(3), (15)] - ordered by row (0 before 2)
- Column 1: [(20)]
- Column 2: [(7)]

**Step 3: Grouping by Column**

Iterate through the sorted list and group by column:

- Process (-1, 1, 9): Column -1 is new, create group [9]
- Process (0, 0, 3): Column 0 is new, create group [3]
- Process (0, 2, 15): Column 0 continues, add to current group [3, 15]
- Process (1, 1, 20): Column 1 is new, create group [20]
- Process (2, 2, 7): Column 2 is new, create group [7]

**Final Result:** `[[9], [3, 15], [20], [7]]`

This represents the vertical order traversal from left to right, with nodes in each column ordered from top to bottom.

## CODE

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        // Store nodes with their coordinates: (column, row, value)
        vector<tuple<int, int, int>> nodesWithCoordinates;
      
        // DFS function to traverse the tree and collect node information
        // Parameters: current node, row index, column index
        function<void(TreeNode*, int, int)> dfs = [&](TreeNode* node, int row, int col) {
            // Base case: if node is null, return
            if (!node) {
                return;
            }
          
            // Store current node's information (column first for sorting)
            nodesWithCoordinates.emplace_back(col, row, node->val);
          
            // Traverse left subtree: row increases by 1, column decreases by 1
            dfs(node->left, row + 1, col - 1);
          
            // Traverse right subtree: row increases by 1, column increases by 1
            dfs(node->right, row + 1, col + 1);
        };
      
        // Start DFS from root at position (0, 0)
        dfs(root, 0, 0);
      
        // Sort nodes by column first, then by row, then by value
        // This is automatically handled by tuple comparison
        sort(nodesWithCoordinates.begin(), nodesWithCoordinates.end());
      
        // Build the result by grouping nodes with the same column
        vector<vector<int>> result;
        int previousColumn = -2000;  // Initialize with value outside possible range
      
        // Process each node in sorted order
        for (auto [currentColumn, currentRow, nodeValue] : nodesWithCoordinates) {
            // If we encounter a new column, create a new group
            if (currentColumn != previousColumn) {
                previousColumn = currentColumn;
                result.emplace_back();
            }
          
            // Add the node value to the current column group
            result.back().push_back(nodeValue);
        }
      
        return result;
    }
};
```

# Solution 2
**Approach:**

Here will insert the tree data into the map of PriorityQueues with rows and columns vertically. So while polling the elements from PriorityQueue it will pop out the elements from the leftmost column to towards right vertically.

1. Break the tree into rows and columns as explained in the problem.

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*PHxV3FF0M5azshjY_cJIVA.jpeg)

Fig-1

2. Now let’s convert the above diagram by pushing it into the nested Map (with TreeMap & Priority Queue) which holds, row, columns, and row-column-wise data. i.e

Press enter or click to view image in full size

![](https://miro.medium.com/v2/resize:fit:700/1*q7FX8YBjdIOh5iCyGBylpA.jpeg)

Fig-2

If you observe **Fig-1** & **Fig-2,** We are constructing the map using BFS traversal i.e row by row from the given tree.

3. Now after constructing the Map we can iterate it and poll the elements from the **PriorityQueue (MinHeap by default).**

Here PriorityQueue will give us traversing from the left-most column. (-2) to right most (2) vertically.

so the **column order** vs **values** :

[-2, -1, 0, 1,2 ]← — — — — — — → [[4],[2],[1,5,6],[3],[7]]

which is a required Output.

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right)
 *         : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    // column -> row -> min heap of values
    map<int, map<int, priority_queue<int, vector<int>, greater<int>>>> mp;

    vector<vector<int>> verticalTraversal(TreeNode* root) {
        dfs(root, 0, 0);

        vector<vector<int>> ans;

        for (auto &col : mp) {
            vector<int> vertical;

            for (auto &row : col.second) {
                auto &pq = row.second;

                while (!pq.empty()) {
                    vertical.push_back(pq.top());
                    pq.pop();
                }
            }

            ans.push_back(vertical);
        }

        return ans;
    }

private:
    void dfs(TreeNode* root, int row, int col) {
        if (!root) return;

        mp[col][row].push(root->val);

        dfs(root->left, row + 1, col - 1);
        dfs(root->right, row + 1, col + 1);
    }
};
```