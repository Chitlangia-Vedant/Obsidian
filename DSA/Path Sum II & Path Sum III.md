# [113. Path Sum II](https://leetcode.com/problems/path-sum-ii/)

Given the `root` of a binary tree and an integer `targetSum`, return _all **root-to-leaf** paths where the sum of the node values in the path equals_ `targetSum`_. Each path should be returned as a list of the node **values**, not node references_.

A **root-to-leaf** path is a path starting from the root and ending at any leaf node. A **leaf** is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
**Output:** [[5,4,11,2],[5,8,4,5]]
**Explanation:** There are two paths whose sum equals targetSum:
5 + 4 + 11 + 2 = 22
5 + 8 + 4 + 5 = 22

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

**Input:** root = [1,2,3], targetSum = 5
**Output:** []

**Example 3:**

**Input:** root = [1,2], targetSum = 0
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

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
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        
    }
};
```

# Solution
## Intuition

A valid answer must be a **root-to-leaf** path whose values add up to `targetSum`.

For every node:

- Include its value in the path.
- Reduce the remaining target by the node's value.
- Recursively search both left and right subtrees.
- Any valid path returned by a child is extended by adding the current node to its front.

The recursion naturally builds complete paths from the leaf back to the root.

---
## Algorithm

1. If the current node is `nullptr`, no path exists.
2. If the current node is a leaf:
   - If its value equals the remaining target, return a path containing only this node.
   - Otherwise, return an empty list.
3. Recursively search:
   - Left subtree with `targetSum - root->val`
   - Right subtree with `targetSum - root->val`
4. Merge all valid paths returned from both subtrees.
5. Prepend the current node's value to every returned path.
6. Return the resulting list.

---
## Dry Run

Target:

```
22
```

Tree:

```
        5
      /   \
     4     8
    /     / \
   11    13  4
  /  \       / \
 7    2     5   1
```

---

### Root = 5

Remaining target:

```
22 - 5 = 17
```

Search both children.

---
### Left Child = 4

Remaining target:

```
17 - 4 = 13
```

Go to node 11.

---
### Node = 11

Remaining target:

```
13 - 11 = 2
```

Explore both leaves.

```
7
```

Remaining target:

```
2 - 7 ≠ 0
```

Invalid.

---

```
2
```

Remaining target:

```
2 - 2 = 0
```

Valid leaf.

Return

```
[[2]]
```

Backtrack.

Node 11 prepends itself.

```
[[11,2]]
```

Node 4 prepends itself.

```
[[4,11,2]]
```

Node 5 prepends itself.

```
[[5,4,11,2]]
```

---
### Right Subtree

The recursion eventually discovers

```
5
→
[[5]]
```

Backtracking produces

```
[[4,5]]
```

then

```
[[8,4,5]]
```

finally

```
[[5,8,4,5]]
```

---
Final answer

```
[
 [5,4,11,2],
 [5,8,4,5]
]
```
## CODE (Mine)

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
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if (root == nullptr)
            return {};

        if (root->left == nullptr && root->right == nullptr) {
            if (targetSum == root->val)
                return {{root->val}};
            return {};
        }
        vector<vector<int>> res;
        auto left = pathSum(root->left, targetSum - root->val);
        if (!left.empty())
            res.insert(res.end(),left.begin(),left.end());
        auto right = pathSum(root->right, targetSum - root->val);
        if (!right.empty())
            res.insert(res.end(),right.begin(),right.end());
        for (auto &x : res) {
            x.insert(x.begin(), root->val);
        }
        return res;
    }
};
```

# [437. Path Sum III](https://leetcode.com/problems/path-sum-iii/)

Given the `root` of a binary tree and an integer `targetSum`, return _the number of paths where the sum of the values along the path equals_ `targetSum`.

The path does not need to start or end at the root or a leaf, but it must go downwards (i.e., traveling only from parent nodes to child nodes).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/pathsum3-1-tree.jpg)

**Input:** root = [10,5,-3,3,2,null,11,3,-2,null,1], targetSum = 8
**Output:** 3
**Explanation:** The paths that sum to 8 are shown.

**Example 2:**

**Input:** root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
**Output:** 3

**Constraints:**

- The number of nodes in the tree is in the range `[0, 1000]`.
- `-109 <= Node.val <= 109`
- `-1000 <= targetSum <= 1000`

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
    int pathSum(TreeNode* root, int targetSum) {
        
    }
};
```

# Solution
## Intuition

A path can start at **any node**, but it must always move **downward**.

Instead of trying every possible starting node, we use the idea of **prefix sums**.

For every node, let:

```
prefixSum = sum of values from the root to the current node
```

Suppose the current prefix sum is:

```
currentSum
```

If there was an earlier prefix sum equal to:

```
currentSum - targetSum
```

then the nodes between those two positions form a path whose sum is exactly `targetSum`.

So while traversing the tree, we keep a hash map storing how many times each prefix sum has appeared on the current root-to-node path.

---
## Prefix Sum Idea

Suppose the current path is

```
10 → 5 → 3
```

Prefix sums:

```
10
15
18
```

Target:

```
8
```

At node `3`:

```
currentSum = 18
```

We need

```
18 - 8 = 10
```

Since prefix sum `10` already exists (at the root),

the path

```
5 → 3
```

has sum

```
18 - 10 = 8
```

So one valid path is found.

---
## Algorithm

1. Maintain:
   - `sum` = current prefix sum.
   - Hash map `s` storing the frequency of every prefix sum seen on the current path.
2. Initialize:

```cpp
s[0] = 1;
```

This allows paths that start from the root to be counted.

3. Visit a node:
   - Add its value to `sum`.
   - The number of valid paths ending at this node is:

```cpp
s[sum - target]
```

4. Store the current prefix sum:

```cpp
s[sum]++;
```

5. Continue DFS for both children.

In this implementation, the hash map is **passed by value**, so every recursive call receives its own independent copy. Because of this, no explicit backtracking (`s[sum]--`) is required.

---
## Dry Run

Tree

```
      10
     /  \
    5   -3
   / \    \
  3   2   11
```

Target

```
8
```

---
### Start

```
s = {0 : 1}
```

Visit

```
10
```

```
sum = 10
```

Need

```
10 - 8 = 2
```

Not present.

Store

```
s = {0,10}
```

---
### Move to 5

```
sum = 15
```

Need

```
15 - 8 = 7
```

Not present.

Store

```
15
```

---
### Move to 3

```
sum = 18
```

Need

```
18 - 8 = 10
```

Prefix `10` exists once.

Found one path:

```
5 → 3
```

Store

```
18
```

Continue DFS.

---
### Visit Other Nodes

Whenever

```
sum - target
```

already exists in the map,

every occurrence represents a different starting point of a valid downward path.

All such paths are counted.
## CODE (Mine)

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
    int target;
    int pathSum(TreeNode* root, int targetSum) {
        if(root==nullptr) return 0;
        target=targetSum;
        unordered_map<long long,int> s;
        s[0]++;
        long long sum=root->val;
        int x=s.contains(sum-target)?s[sum-target]:0;
        
        s[sum]++;
        
        return x+dfs(root->left,sum,s)+dfs(root->right,sum,s);
        
        
    }
    int dfs(TreeNode* root, long long sum, unordered_map<long long,int> s){
        if(root==nullptr) return 0;
        if(root->right==nullptr&&root->left==nullptr){
            sum+=root->val;
            return s.contains(sum-target)?s[sum-target]:0;
        } 
        
        sum+=root->val;
        int x=s.contains(sum-target)?s[sum-target]:0;
        s[sum]++;
        return x+dfs(root->left,sum,s)+dfs(root->right,sum,s);
    }
};
```