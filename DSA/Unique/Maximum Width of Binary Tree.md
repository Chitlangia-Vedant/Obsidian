# [662. Maximum Width of Binary Tree](https://leetcode.com/problems/maximum-width-of-binary-tree/)

Given the `root` of a binary tree, return _the **maximum width** of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of a **32-bit** signed integer.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

**Input:** root = [1,3,2,5,3,null,9]
**Output:** 4
**Explanation:** The maximum width exists in the third level with length 4 (5,3,null,9).

**Example 2:**

![](https://assets.leetcode.com/uploads/2022/03/14/maximum-width-of-binary-tree-v3.jpg)

**Input:** root = [1,3,2,5,null,null,9,6,null,7]
**Output:** 7
**Explanation:** The maximum width exists in the fourth level with length 7 (6,null,null,null,null,null,7).

**Example 3:**

![](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

**Input:** root = [1,3,2,5]
**Output:** 2
**Explanation:** The maximum width exists in the second level with length 2 (3,2).

**Constraints:**

- The number of nodes in the tree is in the range `[1, 3000]`.
- `-100 <= Node.val <= 100`

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
    int widthOfBinaryTree(TreeNode* root) {
        
    }
};
```

# Solution: 1. Breadth First Search

## Intuition

The width of a level is defined by the distance between the leftmost and rightmost non-null nodes, including any null nodes in between. To measure this, we assign each node a position number: the root is `1`, and for any node at position `p`, its left child is at `2 * p` and right child at `2 * p + 1`. Using BFS, we traverse level by level and compute the width as the difference between the last and first position on each level, plus one.

## Algorithm

1. Initialize `res = 0` and a queue with `(root, 1, 0)` representing `(node, position, level)`.
2. Track `prevLevel` and `prevNum` to record the first position on each level.
3. While the queue is not empty:
    - Dequeue `(node, num, level)`.
    - If `level > prevLevel`, update `prevLevel` and `prevNum` to the current level and position.
    - Update `res = max(res, num - prevNum + 1)`.
    - Enqueue the left child with position `2 * num` and the right child with `2 * num + 1`, both at `level + 1`.
4. Return `res`.

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
    int widthOfBinaryTree(TreeNode* root) {
        if (!root) return 0;

        int res = 0;
        queue<tuple<TreeNode*, uint, int>> q; // [node, num, level]
        q.push({root, 1, 0});
        int prevLevel = 0, prevNum = 1;

        while (!q.empty()) {
            auto [node, num, level] = q.front();
            q.pop();

            if (level > prevLevel) {
                prevLevel = level;
                prevNum = num;
            }

            res = max(res, int(num - prevNum) + 1);
            if (node->left) {
                q.push({node->left, 2 * num, level + 1});
            }
            if (node->right) {
                q.push({node->right, 2 * num + 1, level + 1});
            }
        }

        return res;
    }
};
```
## Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---
# Solution: 2. Breadth First Search (Optimal)
## Intuition

Position numbers can grow large in deep skewed trees, potentially causing overflow. To prevent this, we normalize positions at each level by subtracting the starting position of that level. This keeps the numbers small while still correctly computing the width as the difference between the rightmost and leftmost positions on each level.

## Algorithm

1. Initialize `res = 0` and a queue with `(root, 0)`.
2. While the queue is not empty:
    - Record `start` as the position of the first node in the current level.
    - For each node in the current level:
        - Compute `curNum = num - start` to normalize.
        - Update `res = max(res, curNum + 1)`.
        - Enqueue children with positions `2 * curNum` and `2 * curNum + 1`.
3. Return `res`.
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
    int widthOfBinaryTree(TreeNode* root) {
        int res = 0;
        queue<pair<TreeNode*, uint>> q;
        q.push({root, 0});

        while (!q.empty()) {
            int start = q.front().second;

            for (int i = q.size(); i > 0; --i) {
                auto [node, num] = q.front();
                q.pop();
                uint curNum = num - start;
                res = max(res, int(curNum) + 1);
                if (node->left) {
                    q.push({node->left, 2 * curNum});
                }
                if (node->right) {
                    q.push({node->right, 2 * curNum + 1});
                }
            }
        }

        return res;
    }
};
```
## Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---
# Solution:3. Depth First Search

## Intuition

We can also solve this with DFS by recording the first position encountered at each level. As we traverse, whenever we visit a node, we check if its level has been seen before. If not, we record its position as the first for that level. The width at any node is computed as the difference from the first position on its level. We normalize child positions to avoid overflow.

## Algorithm

1. Maintain a map `first` where `first[level]` stores the position of the first node visited at that level.
2. Define `dfs(node, level, curNum)`:
    - If `node` is null, return.
    - If `level` not in `first`, set `first[level] = curNum`.
    - Update `res = max(res, curNum - first[level] + 1)`.
    - Recurse on left child with `level + 1` and position `2 * (curNum - first[level])`.
    - Recurse on right child with `level + 1` and position `2 * (curNum - first[level]) + 1`.
3. Call `dfs(root, 0, 0)` and return `res`.

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
    unordered_map<int, unsigned long long> first;

public:
    int widthOfBinaryTree(TreeNode* root) {
        unsigned long long res = 0;

        dfs(root, 0, 0, res);
        return int(res);
    }

private:
    void dfs(TreeNode* node, int level, unsigned long long num, unsigned long long& res) {
        if (!node) {
            return;
        }

        if (!first.count(level)) {
            first[level] = num;
        }

        res = max(res, num - first[level] + 1);
        dfs(node->left, level + 1, 2 * (num - first[level]), res);
        dfs(node->right, level + 1, 2 * (num - first[level]) + 1, res);
    }
};
```
## Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)