# [110. Balanced Binary Tree](https://leetcode.com/problems/balanced-binary-tree/)

Given a binary tree, determine if it is **height-balanced**.

Height-Balanced
	A **height-balanced** binary tree is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

**Input:** root = [1,2,2,3,3,null,null,4,4]
**Output:** false

**Example 3:**

**Input:** root = []
**Output:** true

**Constraints:**

- The number of nodes in the tree is in the range `[0, 5000]`.
- `-104 <= Node.val <= 104`

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
    public boolean isBalanced(TreeNode root) {
        
    }
}
```

# Solution:

## Approach:

Keep recurring into left and right subtrees and compare if 
abs(leftHeight-rightHeight) > 1: return false
else returns true

## CODE:

```Java
class Solution {
    
    public boolean result = true;

    public boolean isBalanced(TreeNode root) 
    {
        maxHeight(root);
        return result;
    }

    public int maxHeight(TreeNode root)
    {
        if (result == false)
            return 0;

        if (root == null)
            return 0;

        int leftHeight = maxHeight(root.left);  
        int rightHeight = maxHeight(root.right);

        // Not Height Balanced
        if (Math.abs(leftHeight-rightHeight)> 1)
            result = false;

        // Height Balanced  
        return 1 + Math.max(leftHeight,rightHeight);
    }
}
```

TC: O(N)
SC: O(N) - Auxiliary Space