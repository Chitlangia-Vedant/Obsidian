# [530. Minimum Absolute Difference in BST](https://leetcode.com/problems/minimum-absolute-difference-in-bst/)

Given the `root` of a Binary Search Tree (BST), return _the minimum absolute difference between the values of any two different nodes in the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst1.jpg)

**Input:** root = [4,2,6,1,3]
**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/05/bst2.jpg)

**Input:** root = [1,0,48,null,null,12,49]
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[2, 104]`.
- `0 <= Node.val <= 105`

**Note:** This question is the same as 783: [https://leetcode.com/problems/minimum-distance-between-bst-nodes/](https://leetcode.com/problems/minimum-distance-between-bst-nodes/)

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
    public int getMinimumDifference(TreeNode root) {
        
    }
}
```

# Solution:

InOrder Traversal of BST -----> Sorted list of Values

Minimum Absolute Difference in Sorted Array: Between Adjacent Values

Maximum Absolute Difference in Sorted Array: Between First and Last Values


[4 2 6 1 3]
After Sorting:

[1 2 3 4 6]
Min Diff = 1,2 = 2,3 = 3,4 = 1
Max Diff = 1,6 = 5

Note:

(1) If Adjacent values difference is Required ------> No need to Store diff in Array

diff = curr value - prev value

Can be done by a variable


(2) If First and Last Values Difference is Required ------> No need to Store diff in Array

diff = last value - first value 

Can be done by 2 variables

Approach:

(1) InOrder Traversal
(2) Difference of Adjacent Values
(3) Minimum Absolute Value Difference - Ans

# CODE:

```Java
class Solution {
    
    // Global because Recursive function will reuse same value everytime
    int min_diff = Integer.MAX_VALUE; // Ans: Min Diff between any 2 Nodes
    int curr_diff = -1; // Min Diff between any 2 ADJACENT Nodes in Inorder Tarversal

    public int getMinimumDifference(TreeNode root) 
    {
        // Inorder: Left - Root - Right
        
        // Recur on Left
        if (root.left!=null)
            getMinimumDifference(root.left);
        
        if (curr_diff >= 0)
            min_diff = Math.min(min_diff, root.val - curr_diff);
        
        // Hold previous value
        // root.val - curr_diff -> curr - prev value
        curr_diff = root.val;

        // System.out.println("Diff: " + min_diff);
        // System.out.println("Current Node: " + curr_diff );
        
        // Recur on Right        
        if (root.right!=null)
            getMinimumDifference(root.right);
            
        return min_diff;        
    }
}
```

TC - O(N)
SC - O(N)
# CODE

```cpp
int min_dif = INT_MAX, val = -1;    
int getMinimumDifference(TreeNode* root) {
    if (root->left != NULL) 
        getMinimumDifference(root->left);
    if (val >= 0) 
        min_dif = min(min_dif, root->val - val);
    val = root->val;
    if (root->right != NULL) 
        getMinimumDifference(root->right);
    return min_dif;
}
```