# [101. Symmetric Tree](https://leetcode.com/problems/symmetric-tree/)

Given the `root` of a binary tree, _check whether it is a mirror of itself_ (i.e., symmetric around its center).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree1.jpg)

**Input:** root = [1,2,2,3,4,4,3]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/symtree2.jpg)

**Input:** root = [1,2,2,null,3,null,3]
**Output:** false

**Constraints:**

- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`															
**Follow up:** Could you solve it both recursively and iteratively?

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
    public boolean isSymmetric(TreeNode root) {
        
    }
}
```

# Solution:
# Approach

- `areMirror(root.left,root.right)`
	- `root.left.value==root.right.value`
	- `areMirror(root.left.left,root.right.right)`
	- `areMirror(root.left.right.value,root.right.left.value)`
# CODE

```Java
class Solution {
    public boolean isSymmetric(TreeNode root) 
    {
        if (root == null)
            return true;
        
        return areMirror(root.left, root.right);
    }
    
    public boolean areMirror(TreeNode a, TreeNode b)
    {
	// Both Empty - true
	if (a == null && b==null)
		return true;

	// One is Empty, Other is Not Empty  - false
	if (a == null || b==null)
		return false;

	// Both Non-Empty - 3 Conditions
	return (a.val == b.val) && areMirror(a.left, b.right)  && areMirror(a.right, b.left);
    }

}
```

TC - O(N)
SC - O(N)