# [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

According to the [definition of LCA on Wikipedia](https://en.wikipedia.org/wiki/Lowest_common_ancestor): “The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in `T` that has both `p` and `q` as descendants (where we allow **a node to be a descendant of itself**).”

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
**Output:** 3
**Explanation:** The LCA of nodes 5 and 1 is 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

**Input:** root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
**Output:** 5
**Explanation:** The LCA of nodes 5 and 4 is 5, since a node can be a descendant of itself according to the LCA definition.

**Example 3:**

**Input:** root = [1,2], p = 1, q = 2
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[2, 105]`.
- `-109 <= Node.val <= 109`
- All `Node.val` are **unique**.
- `p != q`
- `p` and `q` will exist in the tree.

```Java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
    }
}
```

# LCA: Lowest Common Ancestor

"The lowest common ancestor is defined between two nodes p and q as the lowest node in T that has both p and q as descendants (where we allow a node to be a descendant of itself)."

```
		1
	2       3	
 4    5   6   7	
```

LCA(4,5)
Ancestors of 4: 1, 2, 4
Ancestors of 5: 1, 2, 5

Common Ancestors of (4,5) = 1,2
LCA (4,5) = 2

LCA(4,7)
Ancestors of 4: 1, 2, 4
Ancestors of 7: 1, 3, 7

Common Ancestors of (4,7) = 1
LCA (4,7) = 1

LCA(2,5)
Ancestors of 2: 1, 2
Ancestors of 5: 1, 2, 5

Common Ancestors = 1, 2
LCA(2,5) = 2

# Solution:

## (A) Brute Force to find LCA:

- Path from root to n1
- Path from root to n2
- Last Common Element in the Path: LCA

Eg:
```
		 1
	  2     3  
	4  5   6  7  
```

(1) LCA(4,5) = 2
1 and 2 both are COMMON Ancestor, but LCA is 2

Root to 4 Path: 1 2 4
Root to 5 Path: 1 2 5

Last Common Element: 2 - LCA

(2) LCA(4,6) = 1
Both are in different subtrees, LCA: root: 1

Root to 4 Path: 1 2 4
Root to 6 Path: 1 3 6

Last Common Element: 1 - LCA

Complexity:
- Root to n1: Store in arr1[]
- Root to n2: Store in arr2[]
- Check for last common value in arr1[] and arr2[]


Drawback:
- 2 Iterations of Tree
- Extra Space for two arrays

Can it be solved in Single Iteration? - YES
## (B) Optimised Solution: Single Traversal

Eg:

```
		 1
	  2     3  
	4  5   6  7  
```


(1) LCA(4,5) = 2
1 and 2 both are COMMON Ancestor, but LCA is 2

(2) LCA(4,6) = 1
Both are in different subtrees, LCA: root: 1

(3) LCA(3,4) = 1
Both are in different subtrees, LCA: root: 1

(4) LCA(2,4) = 2
(They Belong to same Subtree)

Case-1: If n1 || n2 matches root, 
        LCA: root 
Eg: (1,6)

If None of them is root, means both of them are children
Recur for left and right subtrees

Case-2: If n1 and n2 lies in left and right subtrees, LCA = root 
Eg: (4,7)

Case-3: If n1 and n2 belong to left subtree, LCA would be in left Subtree
Eg: (2,4)

Case-4: If n1 and n2 belong to right subtree, LCA would be in right Subtree
Eg: (3,7)

## CODE:

```Java
class Solution {
    
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) 
    {

  // Base Case
  if (root == null)
    return null;

  // Case 1: If n1 || n2 matches root, LCA = root (Eg: 1,6)
  if (root.val == p.val || root.val == q.val)
    return root;

  // If Not, Then recur for left and right subtrees
  TreeNode left_lca = lowestCommonAncestor(root.left, p, q);
  TreeNode right_lca = lowestCommonAncestor(root.right, p, q);


 // Case 2: If n1 and n2 lies in left and right subtrees, LCA = root (Eg: 4,7)
  // If n1 and n2 lies in left and right subtree respectively, then left_lca and right_lca BOTH would be Non-Null

  if (left_lca!=null && right_lca!=null)
    return root;


  //  Case 3: If n1 and n2 belong to left subtree, LCA would be in Left Subtree (Eg: 2,4)
  if (left_lca != null)
    return left_lca;

  //  Case 4: If n1 and n2 belong to right subtree, LCA would be in Right Subtree (Eg: 3,7)
  else
    return right_lca;        
    }
}
```

TC: O(N)
SC: O(N)