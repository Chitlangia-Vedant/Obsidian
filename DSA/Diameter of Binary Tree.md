# [543. Diameter of Binary Tree](https://leetcode.com/problems/diameter-of-binary-tree/)

Given the `root` of a binary tree, return _the length of the **diameter** of the tree_.

The **diameter** of a binary tree is the **length** of the longest path between any two nodes in a tree. This path may or may not pass through the `root`.

The **length** of a path between two nodes is represented by the number of edges between them.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

**Input:** root = [1,2,3,4,5]
**Output:** 3
**Explanation:** 3 is the length of the path [4,2,1,3] or [5,2,1,3].

**Example 2:**

**Input:** root = [1,2]
**Output:** 1

**Constraints:**

- The number of nodes in the tree is in the range `[1, 104]`.
- `-100 <= Node.val <= 100`

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
    int diameterOfBinaryTree(TreeNode* root) {
        
    }
};
```

# Solution 

## Approach

```
      1
     / \
    2   3
   / \
  4   5
```

---

◽️ A definition of diameter

The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

---

Simply, by comparing the length of paths from end to end, we can obtain the longest path. Using DFS, we can increment the length by `1` as we backtrack, which should allow us to find the longest path.

Let's move down to node `4` from root node.

```python
     n1
     / \
   n2  n3
   / \
#n4  n5
0/ \0

# is current node
n is node
```

From node `4`, there is no children, so length should be `0`. Let's go back to node `2`.

But before that, let's calculate `current max length`. Current max length should be `current result` or `left + right`.

```python
res = max(res, l + r)
res = max(0, 0 + 0)
= 0
```

After that, we should add `1` to longer length of path between left or right to keep the longest length one way path, **because the diameter of a binary tree is the length of the longest path between any two nodes in a tree. We will have one node from subtree from child and have the other node when we go back to parent node.**

Add `1` to longer length between left or right. In this case both side are `0`, so current length should be

```sql
1 + max(l, r)
= 1 + max(0, 0)
= 1 (= between node 2 and node 4)
```

We do the same thing for right side of node `2`

```python
     n1
     / \
  #n2  n3
  1/ \
 n4  n5
0/ \00/\0
```

```python
     n1
     / \
   n2  n3
  1/ \
 n4  n5#
0/ \00/\0

res = max(res, l + r)
res = max(0, 0 + 0)
= 0
```

Let's go back to node `2` from node `5`

```python
     n1
     / \
   n2  n3
  1/ \1
 n4  n5#
0/ \00/\0

1 + max(l, r)
= 1 + max(0, 0)
= 1(= between node 2 and node 5)

     n1
     / \
  #n2  n3
  1/ \1
 n4  n5
0/ \00/\0
```

Let's move back to root node from node `2`. But before that let's calculate max length.

```python
     n1
     / \
  #n2  n3
  1/ \1
 n4  n5
0/ \00/\0

res = max(res, l + r)
res = max(0, 1 + 1)
= 2 (4 → 2 → 5 or 5 → 2 → 4)
```

```python
    #n1
    2/ \
   n2  n3
  1/ \1
 n4  n5
0/ \00/\0

1 + max(l, r)
= 1 + max(1, 1)
= 2 (=between node 1 and node 4 or 5)
```

Let's go right side of node `1`.

```python
     n1
    2/ \
   n2  #n3
  1/ \1 0/\0
 n4  n5
0/ \00/\0

res = max(res, l + r)
res = max(2, 0 + 0)
= 2 (4 → 2 → 5 or 5 → 2 → 4)
```

Let's go back to node `1`.

```python
    #n1
   2 / \ 1
   n2  n3
  1/ \1 0/\0
 n4  n5
0/ \00/\0

1 + max(l, r)
= 1 + max(0, 0)
= 1 (=between node 1 and node 3)
```

In the end, calculate max length at root node.

```python
res = max(res, l + r)
res = max(2, 2 + 1)
= 3 

4 → 2 → 1 → 3 or
5 → 2 → 1 → 3 or
3 → 1 → 2 → 4 or
3 → 1 → 2 → 5
```

I wrote this while half-asleep, so please let me know if there are any mistakes.

## Complexity
- Time complexity: O(n)
- Space complexity: O(h) or O(n)

## Step by Step Algorithm

### Code Part 1: Initializing Variables

```python
res = 0
```

- **Explanation**:
    - `res` is a variable to store the current maximum diameter found during the traversal. It is initialized to 0.

---

### Code Part 2: Inner Function for Depth-First Search (DFS)

```python
def dfs(root):
    if not root:
        return 0
```

- **Explanation**:
    - A helper function `dfs` is defined to calculate the depth of the binary tree using recursion.
    - If `root` is `None` (i.e., an empty node), the function returns `0`, as the depth at this point is 0.

---

### Code Part 3: Recursive Calls to Compute Depths

```python
l = dfs(root.left)
r = dfs(root.right)
```

- **Explanation**:
    - The function calls itself recursively for the left child (`root.left`) and the right child (`root.right`) of the current node.
    - `l` stores the depth of the left subtree, and `r` stores the depth of the right subtree.

---

### Code Part 4: Update Diameter

```python
nonlocal res
res = max(res, l + r)
```

- **Explanation**:
    - The `nonlocal` keyword is used to modify the `res` variable declared outside the `dfs` function.
    - The diameter at the current node is calculated as `l + r` (the sum of the depths of the left and right subtrees).
    - `res` is updated to the maximum of its current value and the diameter at the current node.

---

### Code Part 5: Return Depth

```python
return 1 + max(l, r)
```

- **Explanation**:
    - The function returns the depth of the subtree rooted at the current node.
    - The depth is calculated as `1 + max(l, r)`, where `1` accounts for the current node, and `max(l, r)` gives the maximum depth of its subtrees.

---

### Code Part 6: Initiate DFS and Return Result

```python
dfs(root)
return res
```

- **Explanation**:
    - The `dfs` function is called with the root node to calculate the diameter of the tree.
    - After the traversal, the value of `res` (the maximum diameter found) is returned as the final result.

## CODE (Mine)

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
    int diameterOfBinaryTree(TreeNode* root) {
        int diameter = 0;
        heightOfTree(root, diameter);
        return diameter;
    }

    int heightOfTree(TreeNode* node, int& diameter) {
        if (node == nullptr) {
            return 0;
        }
        int leftHeight = heightOfTree(node->left, diameter);
        int rightHeight = heightOfTree(node->right, diameter);

        // Update the diameter
        diameter = std::max(diameter, leftHeight + rightHeight);

        return std::max(leftHeight, rightHeight) + 1;
    }

};
```