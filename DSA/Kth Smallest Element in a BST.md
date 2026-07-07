# [230. Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/)

Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _smallest value (**1-indexed**) of all the values of the nodes in the tree_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree1.jpg)

**Input:** root = [3,1,4,null,2], k = 1
**Output:** 1

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/28/kthtree2.jpg)

**Input:** root = [5,3,6,2,4,null,null,1], k = 3
**Output:** 3

**Constraints:**

- The number of nodes in the tree is `n`.
- `1 <= k <= n <= 104`
- `0 <= Node.val <= 104`

**Follow up:** If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

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
    public int kthSmallest(TreeNode root, int k) {
        
    }
}
```

# Solution:

## (1) Brute Force:

Heap:
- Max-Heap
- Min-Heap

Use Heap (Min-Heap - Kth Min) (Max Heap - Kth Max) and pop K Times

Time and Space Complexity will increase due to Heap/PQ

TC: O(NlogN), Optimal: O(NlogK) (K Elements in a a Heap at one time)
SC: O(N)

## (2) InOrder Traversal:

InOrder Traversal of BST -------> Sorted Array

Store in Array:
Kth Smallest Value = return arr[K-1]
Kth Largest Value = return arr[N-K+1]

TC: O(N) - InOrder Traversal
SC: O(N) - Store in Array


Follow up: 
If the BST is modified often (i.e., we can do insert and delete operations) and you need to find the kth smallest frequently, how would you optimize?

N = 10^9
K = 2

InOrder Traversal: 10^9 Iterations
Storage: arr[10^9]

## (3) Most Optimised Solution:

Inside InOrder Traversal:

Initialise count = 0

During InOrder Traversal:

```
	if count == K:
		return root.data
```

TC: O(K), 1 <= K <= N
SC: O(1) - In Memory

# CODE:

```Java
class Solution 
{
    int count = 0;
    int result = -1;

    public int kthSmallest(TreeNode root, int k) 
    {
        traverse(root, k);
        return result;
    }

    public void traverse(TreeNode root, int k)
    {
        if (root == null)
            return;

         // Inorder: left - root - right
        traverse(root.left, k); // recur left subtree
        count++;

        if (count == k)
        {
            result = root.val;
            return;
        }
        
        traverse(root.right, k); // recur right subtree
    }
}
```



TC: O(K)
SC: O(1) - In Memory
    O(N) - Recursion Stack