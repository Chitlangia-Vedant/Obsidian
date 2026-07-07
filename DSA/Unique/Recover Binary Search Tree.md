# [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)

You are given the `root` of a binary search tree (BST), where the values of **exactly** two nodes of the tree were swapped by mistake. _Recover the tree without changing its structure_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover1.jpg)

**Input:** root = [1,3,null,null,2]
**Output:** [3,1,null,null,2]
**Explanation:** 3 cannot be a left child of 1 because 3 > 1. Swapping 1 and 3 makes the BST valid.

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/28/recover2.jpg)

**Input:** root = [3,1,4,null,null,2]
**Output:** [2,1,4,null,null,3]
**Explanation:** 2 cannot be in the right subtree of 3 because 2 < 3. Swapping 2 and 3 makes the BST valid.

**Constraints:**

- The number of nodes in the tree is in the range `[2, 1000]`.
- `-231 <= Node.val <= 231 - 1`

**Follow up:** A solution using `O(n)` space is pretty straight-forward. Could you devise a constant `O(1)` space solution?

# Solution
## 1. Brute Force

### Intuition

The simplest approach is to try every possible pair of nodes and check if swapping their values produces a valid BST. Since exactly two nodes were swapped, one such pair must restore the tree to its correct state.

For each pair of nodes, we swap their values, validate whether the resulting tree is a valid BST, and either keep the swap (if valid) or revert it (if invalid). While inefficient, this approach guarantees finding the solution by exhaustively checking all possibilities.

### Algorithm

1. Define a helper function `isBST` that validates whether a tree is a valid BST using BFS with min/max bounds for each node.
2. Use a nested DFS approach: the outer DFS (`dfs`) iterates through each node as a potential first swap candidate.
3. For each first candidate, the inner DFS (`dfs1`) iterates through every other node as the second swap candidate.
4. Once a valid swap is found, the tree is corrected in place.

### CODE
Recover Binary Search Tree - DFS with Swap Testing

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
    void recoverTree(TreeNode* root) {
        dfs(root, root, root);
    }

    bool dfs(TreeNode* node1, TreeNode* node2, TreeNode* root) {
        if (!node1) return false;
        if (dfs1(node1, node2, root)) return true;
        return dfs(node1->left, node2, root) || dfs(node1->right, node2, root);
    }

    bool dfs1(TreeNode* node1, TreeNode* node2, TreeNode* root) {
        if (!node2 || node1 == node2) return false;

        swap(node1->val, node2->val);
        if (isBST(root)) return true;
        swap(node1->val, node2->val);

        return dfs1(node1, node2->left, root) || dfs1(node1, node2->right, root);
    }

    bool isBST(TreeNode* node) {
        TreeNode* prev = nullptr;
        return inorder(node, prev);
    }

    bool inorder(TreeNode* node, TreeNode*& prev) {
        if (!node) return true;
        if (!inorder(node->left, prev)) return false;
        if (prev && prev->val >= node->val) return false;
        prev = node;
        return inorder(node->right, prev);
    }
};
```
### Time & Space Complexity

- Time complexity: O($n^2$)
- Space complexity: O(n)

---
## 2. Inorder Traversal

### Intuition

A key property of BSTs is that an inorder traversal produces values in sorted order. When two nodes are swapped incorrectly, this sorted sequence will have one or two "inversions" where a value is greater than the next value.

If the swapped nodes are adjacent in the inorder sequence, there will be exactly one inversion. If they are not adjacent, there will be two inversions. In the first inversion, the larger (out-of-place) node is the first swapped node. In the second inversion (or the same one if only one exists), the smaller node is the second swapped node.

By collecting all nodes during inorder traversal and then scanning for these inversions, we can identify the two swapped nodes and swap their values back.

### Algorithm

1. Perform an inorder traversal of the tree and store all nodes in a list.
2. Scan the list to find inversions (where `arr[i].val > arr[i+1].val`).
3. On the first inversion, mark `node1 = arr[i]` and `node2 = arr[i+1]`.
4. If a second inversion is found, update `node2 = arr[i+1]` (the first swap target is already correct).
5. Swap the values of `node1` and `node2` to restore the BST.

### CODE
Recover Binary Search Tree - Inorder Traversal

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
    void recoverTree(TreeNode* root) {
        vector<TreeNode*> arr;
        inorder(root, arr);

        TreeNode* node1 = nullptr;
        TreeNode* node2 = nullptr;

        for (int i = 0; i < arr.size() - 1; i++) {
            if (arr[i]->val > arr[i + 1]->val) {
                node2 = arr[i + 1];
                if (!node1) node1 = arr[i];
                else break;
            }
        }

        swap(node1->val, node2->val);
    }

    void inorder(TreeNode* node, vector<TreeNode*>& arr) {
        if (!node) return;
        inorder(node->left, arr);
        arr.push_back(node);
        inorder(node->right, arr);
    }
};
```

### Time & Space Complexity

- Time complexity: O(n)
- Space complexity: O(n)

---

## 3. Iterative Inorder Traversal

### Intuition

This approach uses the same logic as the recursive inorder traversal but implements it iteratively using an explicit stack. The advantage is that we can detect inversions on the fly during traversal rather than collecting all nodes first.

By keeping track of the previously visited node, we can immediately detect when the current node's value is less than the previous node's value, signaling an inversion. This allows us to identify the swapped nodes in a single pass through the tree.

### Algorithm

1. Initialize an empty stack and pointers for `node1`, `node2`, `prev`, and `curr`.
2. Traverse the tree iteratively using the stack for inorder traversal.
3. For each visited node, compare it with `prev`. If `prev.val > curr.val`, an inversion is found.
4. On the first inversion, set `node1 = prev` and `node2 = curr`.
5. On the second inversion, update `node2 = curr` and break early.
6. After traversal, swap the values of `node1` and `node2`.

### CODE
Recover Binary Search Tree - In-order Traversal

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
    void recoverTree(TreeNode* root) {
        stack<TreeNode*> stack;
        TreeNode *node1 = nullptr, *node2 = nullptr, *prev = nullptr, *curr = root;

        while (!stack.empty() || curr) {
            while (curr) {
                stack.push(curr);
                curr = curr->left;
            }

            curr = stack.top(); stack.pop();
            if (prev && prev->val > curr->val) {
                node2 = curr;
                if (!node1) node1 = prev;
                else break;
            }
            prev = curr;
            curr = curr->right;
        }

        swap(node1->val, node2->val);
    }
};
```

### Time & Space Complexity

- Time complexity: O(n)
- Space complexity: O(n)

---
## 4. Morris Traversal

### Intuition

Morris traversal allows us to perform inorder traversal without using a stack or recursion, achieving `O(1)` space complexity. The technique works by temporarily modifying the tree structure: for each node with a left child, we find its inorder predecessor and create a temporary link back to the current node.

This temporary threading allows us to return to ancestor nodes after processing the left subtree without needing a stack. We can detect inversions during this traversal just like in the iterative approach, but without the extra space for a stack.

### Algorithm

1. Initialize pointers for `node1`, `node2`, `prev`, and `curr` (starting at root).
2. While `curr` is not null:
    - If `curr` has no left child, process it (check for inversion with `prev`), update `prev`, and move to the right child.
    - If `curr` has a left child, find its inorder predecessor (rightmost node in left subtree).
        - If the predecessor's right pointer is null, create a thread to `curr` and move left.
        - If the predecessor's right pointer points to `curr`, remove the thread, process `curr`, update `prev`, and move right.
3. After traversal, swap the values of `node1` and `node2`.

### CODE
Recover Binary Search Tree - Morris Inorder Traversal

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
    void recoverTree(TreeNode* root) {
        TreeNode* node1 = nullptr;
        TreeNode* node2 = nullptr;
        TreeNode* prev = nullptr;
        TreeNode* curr = root;

        while (curr) {
            if (!curr->left) {
                if (prev && prev->val > curr->val) {
                    node2 = curr;
                    if (!node1) node1 = prev;
                }
                prev = curr;
                curr = curr->right;
            } else {
                TreeNode* pred = curr->left;
                while (pred->right && pred->right != curr) {
                    pred = pred->right;
                }

                if (!pred->right) {
                    pred->right = curr;
                    curr = curr->left;
                } else {
                    pred->right = nullptr;
                    if (prev && prev->val > curr->val) {
                        node2 = curr;
                        if (!node1) node1 = prev;
                    }
                    prev = curr;
                    curr = curr->right;
                }
            }
        }

        swap(node1->val, node2->val);
    }
};
```

### Time & Space Complexity

- Time complexity: O(n)
- Space complexity: O(1)