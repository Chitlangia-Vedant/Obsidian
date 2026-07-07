# [979. Distribute Coins in Binary Tree](https://leetcode.com/problems/distribute-coins-in-binary-tree/)

You are given the `root` of a binary tree with `n` nodes where each `node` in the tree has `node.val` coins. There are `n` coins in total throughout the whole tree.

In one move, we may choose two adjacent nodes and move one coin from one node to another. A move may be from parent to child, or from child to parent.

Return _the **minimum** number of moves required to make every node have **exactly** one coin_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree1.png)

**Input:** root = [3,0,0]
**Output:** 2
**Explanation:** From the root of the tree, we move one coin to its left child, and one coin to its right child.

**Example 2:**

![](https://assets.leetcode.com/uploads/2019/01/18/tree2.png)

**Input:** root = [0,3,0]
**Output:** 3
**Explanation:** From the left child of the root, we move two coins to the root [taking two moves]. Then, we move one coin from the root of the tree to the right child.

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= n <= 100`
- `0 <= Node.val <= n`
- The sum of all `Node.val` is `n`.

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
    int distributeCoins(TreeNode* root) {
        
    }
};
```
# Solution

We traverse childs first (post-order traversal), and return the ballance of coins. For example, if we get '+3' from the left child, that means that the left subtree has 3 extra coins to move out. If we get '-1' from the right child, we need to move 1 coin in. So, we increase the number of moves by 4 (3 moves out left + 1 moves in right). We then return the final ballance: r->val (coins in the root) + 3 (left) + (-1) (right) - 1 (keep one coin for the root). 
![[Pasted image 20260705150421.png]]

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
	int traverse(TreeNode* r, int &moves) {
	  if (r == nullptr) return 0;
	  int left = traverse(r->left, moves), right = traverse(r->right, moves);
	  moves += abs(left) + abs(right);
	  return r->val + left + right - 1;
	}
	int distributeCoins(TreeNode* r, int moves = 0) {
	  traverse(r, moves);
	  return moves;
	}
};
```