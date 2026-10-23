[222. Count Complete Tree Nodes](https://leetcode.com/problems/count-complete-tree-nodes/)

Given the `root` of a **complete** binary tree, return the number of the nodes in the tree.

According to **[Wikipedia](http://en.wikipedia.org/wiki/Binary_tree#Types_of_binary_trees)**, every level, except possibly the last, is completely filled in a complete binary tree, and all nodes in the last level are as far left as possible. It can have between `1` and `2h` nodes inclusive at the last level `h`.

Design an algorithm that runs in less than `O(n)` time complexity.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/14/complete.jpg)

**Input:** root = [1,2,3,4,5,6]
**Output:** 6

**Example 2:**

**Input:** root = []
**Output:** 0

**Example 3:**

**Input:** root = [1]
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5 * 104]`.
- `0 <= Node.val <= 5 * 104`
- The tree is guaranteed to be **complete**.

## Intuition

When counting nodes in a complete binary tree, the straightforward approach would be to visit every node. However, complete trees have enough structure for us to skip large perfect subtrees.

At each subtree, compute two heights:

- `left_height`: follow `left` pointers until reaching `None`
- `right_height`: follow `right` pointers until reaching `None`

If the two heights match, the subtree is a perfect binary tree. A perfect tree with height `h` contains `(2^h) - 1` nodes, so we can return that value immediately.

If the heights differ, the subtree is complete but not perfect, so at least one child subtree may still be perfect. We recursively apply the same logic to the left and right children and add `1` for the current root.

### Solution Approach

The solution uses recursive DFS with a perfect-subtree shortcut.

**Base Case:** When we encounter a `None` node (empty subtree), we return `0` since there are no nodes to count.

Copy

```python
if root is None:
    return 0
```

**Perfect Subtree Check:** For any non-null node, calculate the leftmost and rightmost heights.

Copy

```python
left_height = self.get_left_height(root)
right_height = self.get_right_height(root)
if left_height == right_height:
    return (1 << left_height) - 1
```

If these heights match, the subtree is perfect, and we can compute the answer directly.

**Recursive Case:** If the heights differ, recursively count the two children:

Copy

```python
return 1 + self.countNodes(root.left) + self.countNodes(root.right)
```

The algorithm follows these steps:

1. Start at the root node
2. If the current node is `None`, return `0`
3. Compute leftmost and rightmost heights from the current node
4. If the heights match, return `(2^height) - 1`
5. Otherwise, return `1 + countNodes(root.left) + countNodes(root.right)`

## Example Walkthrough

Let's walk through counting nodes in a small complete binary tree with 6 nodes:

Copy

```
        1
       / \
      2   3
     / \ /
    4  5 6
```

**Step-by-step execution:**

1. **Start at root (1):**
    
    - Leftmost height is `3` (`1 -> 2 -> 4`)
    - Rightmost height is `2` (`1 -> 3`)
    - Heights differ, so the tree is not perfect. Return `1 + countNodes(2) + countNodes(3)`.
2. **Process left subtree (node 2):**
    
    - Leftmost height is `2` (`2 -> 4`)
    - Rightmost height is `2` (`2 -> 5`)
    - Heights match, so this subtree is perfect and contains `(2^2) - 1 = 3` nodes.
3. **Process right subtree (node 3):**
    
    - Leftmost height is `2` (`3 -> 6`)
    - Rightmost height is `1` (`3`)
    - Heights differ, so return `1 + countNodes(6) + countNodes(None)`.
    - Node `6` is perfect with height `1`, so it contributes `1`.
    - `None` contributes `0`.
    - Node `3`'s subtree contributes `1 + 1 + 0 = 2`.
4. **Final calculation:**
    
    - Root's count: `1 + 3 + 2 = 6`

The algorithm skips counting every node in the perfect left subtree individually, which is the key optimization for complete binary trees.

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
    int countNodes(TreeNode* root) {
        if (!root) {
            return 0;
        }

        int leftHeight = getLeftHeight(root);
        int rightHeight = getRightHeight(root);

        if (leftHeight == rightHeight) {
            return (1 << leftHeight) - 1;
        }

        return 1 + countNodes(root->left) + countNodes(root->right);
    }

private:
    int getLeftHeight(TreeNode* node) {
        int height = 0;
        while (node) {
            height++;
            node = node->left;
        }
        return height;
    }

    int getRightHeight(TreeNode* node) {
        int height = 0;
        while (node) {
            height++;
            node = node->right;
        }
        return height;
    }
};

```

## Time and Space Complexity

**Time Complexity:** `O(log^2 n)`, where `n` is the number of nodes in the complete binary tree.

At each recursive level, we compute the leftmost and rightmost heights, which costs `O(log n)` in a complete tree. The recursion descends through at most `O(log n)` levels because perfect subtrees are counted directly, so the total time complexity is `O(log^2 n)`.

**Space Complexity:** `O(log n)`.

A complete binary tree has height `O(log n)`, and the recursive call stack follows at most that many levels.