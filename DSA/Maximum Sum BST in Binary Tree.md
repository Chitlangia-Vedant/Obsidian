# [1373. Maximum Sum BST in Binary Tree](https://leetcode.com/problems/maximum-sum-bst-in-binary-tree/)

Given a **binary tree** `root`, return _the maximum sum of all keys of **any** sub-tree which is also a Binary Search Tree (BST)_.

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/01/30/sample_1_1709.png)

**Input:** root = [1,4,3,2,4,2,5,null,null,null,null,null,null,4,6]
**Output:** 20
**Explanation:** Maximum sum in a valid Binary search tree is obtained in root node with key equal to 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/01/30/sample_2_1709.png)

**Input:** root = [4,3,null,1,2]
**Output:** 2
**Explanation:** Maximum sum in a valid Binary search tree is obtained in a single root node with key equal to 2.

**Example 3:**

**Input:** root = [-4,-2,-5]
**Output:** 0
**Explanation:** All values are negatives. Return an empty BST.

**Constraints:**

- The number of nodes in the tree is in the range `[1, 4 * 104]`.
- `-4 * 104 <= Node.val <= 4 * 104`

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
    int maxSumBST(TreeNode* root) {
        
    }
};
```

# Solution
## Intuition  
  
To determine whether a subtree is a BST, we need more than just its sum.  
  
For every subtree, we need to know:  
  
- Is it a valid BST?  
- What is the sum of all its nodes?  
- What is its minimum value?  
- What is its maximum value?  
  
This information is returned to the parent after processing both children.  
  
Using this bottom-up approach (postorder traversal), each node can decide whether the subtree rooted at itself is a BST and, if so, compute its sum.  
  
While doing this, we continuously update the maximum BST sum found so far.  
  
---  
## Information Returned  
  
For every subtree, the recursive function returns:  
  
```cpp  
struct NodeInfo {  
bool isBST;  
int sum;  
int mn;  
int mx;  
};  
```  
  
Meaning:  
  
- `isBST` → whether the subtree is a BST.  
- `sum` → sum of all nodes in the subtree.  
- `mn` → minimum value in the subtree.  
- `mx` → maximum value in the subtree.  
  
---  
## Base Case  
  
For a `nullptr` node:  
  
```cpp  
{true, 0, INT_MAX, INT_MIN}  
```  
  
Why?  
  
- An empty tree is considered a BST.  
- Its sum is `0`.  
- `INT_MAX` and `INT_MIN` make comparisons always succeed.  
  
Example:  
  
```  
5  
/  
nullptr  
```  
  
Check:  
  
```  
left.mx = INT_MIN < 5  
```  
  
Always true.  
  
Similarly,  
  
```  
5 < right.mn = INT_MAX  
```  
  
is also true.  
  
---  
## Algorithm  
  
For every node:  
  
1. Recursively compute information for the left subtree.  
2. Recursively compute information for the right subtree.  
3. The current subtree is a BST if:  
- left subtree is a BST,  
- right subtree is a BST,  
- `left.mx < root->val`,  
- `root->val < right.mn`.  
4. If valid:  
- Compute the subtree sum.  
- Update the global maximum.  
- Return:  
- `isBST = true`  
- new sum  
- minimum value  
- maximum value  
5. Otherwise:  
- Return a marker indicating this subtree is **not** a BST.  
  
```cpp  
{false, 0, INT_MIN, INT_MAX}  
```  
  
The swapped min/max values ensure that no ancestor can mistakenly treat this subtree as a valid BST.  
  
---  
## Dry Run  
  
Tree:  
  
```  
3  
/ \  
2 5  
/ \  
4 6  
```  
  
---  
### Node 2  
  
Left and right are empty.  
  
```  
BST ✔  
  
sum = 2  
min = 2  
max = 2  
```  
  
Update:  
  
```  
maxSum = 2  
```  
  
---  
### Node 4  
  
```  
BST  
  
sum = 4  
```  
  
Update:  
  
```  
maxSum = 4  
```  
  
---  
### Node 6  
  
```  
BST  
  
sum = 6  
```  
  
Update:  
  
```  
maxSum = 6  
```  
  
---  
### Node 5  
  
Children:  
  
```  
left.max = 4  
right.min = 6  
```  
  
Check:  
  
```  
4 < 5 < 6  
```  
  
BST.  
  
```  
sum = 4 + 6 + 5 = 15  
```  
  
Update:  
  
```  
maxSum = 15  
```  
  
Return:  
  
```  
BST  
sum = 15  
min = 4  
max = 6  
```  
  
---  
### Root 3  
  
Children:  
  
```  
left.max = 2  
right.min = 4  
```  
  
Check:  
  
```  
2 < 3 < 4  
```  
  
BST.  
  
```  
sum = 2 + 15 + 3 = 20  
```  
  
Update:  
  
```  
maxSum = 20  
```  
  
Answer:  
  
```  
20  
```
## CODE (Mine)

```cpp
class Solution {
public:
    int maxSum = 0;

    struct NodeInfo {
        bool isBST;
        int sum;
        int mn;
        int mx;
    };

    int maxSumBST(TreeNode* root) {
        SumBST(root);
        return maxSum;
    }

    NodeInfo SumBST(TreeNode* root) {
        if (root == nullptr)
            return {true, 0, INT_MAX, INT_MIN};

        NodeInfo left = SumBST(root->left);
        NodeInfo right = SumBST(root->right);

        if (left.isBST && right.isBST &&
            left.mx < root->val &&
            root->val < right.mn) {

            int sum = left.sum + right.sum + root->val;
            maxSum = max(maxSum, sum);

            return {
                true,
                sum,
                min(left.mn, root->val),
                max(right.mx, root->val)
            };
        }

        return {false, 0, INT_MIN, INT_MAX};
    }
};
```