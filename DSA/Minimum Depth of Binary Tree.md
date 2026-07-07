# [111. Minimum Depth of Binary Tree](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/12/ex_depth.jpg)

**Input:** root = [3,9,20,null,null,15,7]
**Output:** 2

**Example 2:**

**Input:** root = [2,null,3,null,4,null,5,null,6]
**Output:** 5

**Constraints:**

- The number of nodes in the tree is in the range `[0, 105]`.
- `-1000 <= Node.val <= 1000`

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
    public int minDepth(TreeNode root) {
        
    }
}
```

# Solution:

"The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node."

Edge Case: Skewed Trees


```
	1
	  2
	  	3
	  	  4
	  	    5
```

Leaf Node: 5

OP: 5

Min Depth: 5
Max Depth: 5

```
If Skewed Tree, 
Min Distance from root to leaf = Max Distance from root to leaf

Need: Smaller of Child = min(left, right) - Works fine with Non-Skewed Trees

If skewed tree,
min(left, right) = 0
Either of this value = 0 : min(leftMin, rightMin) == 0


if min(leftMin, rightMin) == 0
	Select ans = max(leftMin, rightMin)
```

# CODE:

```Java
class Solution {
    public int minDepth(TreeNode root) 
    {
        if (root == null)
            return 0;
        
        int leftHeight = minDepth(root.left);
        int rightHeight = minDepth(root.right);

        // Skewed Tree
        if (Math.min(leftHeight, rightHeight) == 0)
            return 1 + Math.max(leftHeight, rightHeight);

        // Non-Skewed Tree
        else
        return 1 + Math.min(leftHeight, rightHeight);
    }
}
```

TC: O(N)
SC: O(N)