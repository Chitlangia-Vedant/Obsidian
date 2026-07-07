# [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/12/tree1.jpg)

**Input:** root = [4,2,7,1,3], val = 2
**Output:** [2,1,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/12/tree2.jpg)

**Input:** root = [4,2,7,1,3], val = 5
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[1, 5000]`.
- `1 <= Node.val <= 107`
- `root` is a binary search tree.
- `1 <= val <= 107`

# Solution:

## Approach:
Binary Search


## CODE
```Java
class Solution {

    // Iterative Code - Binary Search
    // T: O(log N), S: O(1)
    public TreeNode searchBST(TreeNode root, int data) 
    {
        while (root!=null && root.val!=data)
        {
            if (data < root.val)
                root = root.left;
            else
                root = root.right;
        }
        
        return root;
    }


    // Recursive Code - Binary Search
    // T: O(log N), S: O(1)

    public TreeNode searchBST(TreeNode root, int data) 
    {
        if (root == null || root.val == data)
            return root;

        else
        {
            if (data < root.val)
                root = searchBST(root.left, data);

            else 
                root = searchBST(root.right, data);
        }

        return root;    
    }   
    
}
```

TC: O(log N) 
SC: O(1)