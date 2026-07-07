# [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

Given the `root` of a binary tree, return _its maximum depth_.

A binary tree's **maximum depth** is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 3

**Example 2:**

**Input:** root = [1,null,2]
**Output:** 2

**Constraints:**

- The number of nodes in the tree is in the range `[0, 104]`.
- `-100 <= Node.val <= 100`

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    public int maxDepth(TreeNode root) {
        
    }
}
```

# Solution:

## Approach:

Binary Tree: 2 Nodes (Left and Right Subtrees)

For Maximum Depth/Height of Tree
= Maximum of Left and Right Subtrees Height/Depth + 1 (Root Node)
## Formula:

height = 1 + max(root.left, root.right)

```
IP:

		1
	2      3	
		4     5
	  6	

OP: 4
```

## Dry Run:

```
			1 - max(1,3) + 1 = 3+1 = 4 - OP
	2(1)         3 (3)
			4 (2)   5 (1)
	  	  6 (1)	

OP: 4
```

# CODE:

```Java
class Solution 
{
    public int maxDepth(TreeNode root) 
    {
    if (root == null)
        return 0;
        
    int left_height = maxDepth(root.left); // Left Subtree

    int right_height = maxDepth(root.right); // Right Subtree

    return 1 + Math.max(left_height, right_height);
    }
}
```

TC: O(N)
SC: O(N) - Recursion