# [116. Populating Next Right Pointers in Each Node](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

You are given a **perfect binary tree** where all leaves are on the same level, and every parent has two children. The binary tree has the following definition:

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}

Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to `NULL`.

Initially, all next pointers are set to `NULL`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

**Input:** root = [1,2,3,4,5,6,7]
**Output:** [1,#,2,3,#,4,5,6,7,#]
**Explanation:** Given the above perfect binary tree (Figure A), your function should populate each next pointer to point to its next right node, just like in Figure B. The serialized output is in level order as connected by the next pointers, with '#' signifying the end of each level.

**Example 2:**

**Input:** root = []
**Output:** []

**Constraints:**

- The number of nodes in the tree is in the range `[0, 212 - 1]`.
- `-1000 <= Node.val <= 1000`

**Follow-up:**

- You may only use constant extra space.
- The recursive approach is fine. You may assume implicit stack space does not count as extra space for this problem.

# Solution
## 1. Breadth First Search

### Intuition

Level-order traversal naturally groups nodes by their depth, which is exactly what we need to connect nodes on the same level. As we process each level, the next node in the queue is the right neighbor of the current node.

By tracking the size of each level, we can connect all nodes within a level while avoiding connecting the last node of one level to the first node of the next.

### Algorithm

1. If the `root` is `null`, return `null`.
2. Initialize a queue with the `root`.
3. While the queue is not empty:
    - Record the current level size.
    - For each node in the current level:
        - Dequeue the node.
        - If it's not the last node in the level, set its `next` pointer to the front of the queue.
        - Enqueue its `left` and `right` children if they exist.
4. Return the `root`.

### CODE (Mine)

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return nullptr;

        queue<Node*> q;
        q.push(root);

        while (!q.empty()) {
            int levelSize = q.size();
            while (levelSize > 0) {
                Node* node = q.front();
                q.pop();
                if (levelSize > 1) {
                    node->next = q.front();
                }
                if (node->left) {
                    q.push(node->left);
                }
                if (node->right) {
                    q.push(node->right);
                }
                levelSize--;
            }
        }

        return root;
    }
};
```

### Time & Space Complexity
- Time complexity: O(n)
- Space complexity: O(log⁡n)

## 3. Depth First Search (Optimal)

### Intuition

In a perfect binary tree, every node has either zero or two children, and all leaves are at the same level. This structure lets us establish `next` pointers without extra space for tracking.

For any node with children, its `left` child's `next` is always its `right` child. And if the node has a `next` pointer, its `right` child's `next` is the `left` child of the node's `next` neighbor. This recursive pattern connects the entire tree.

### Algorithm

1. If the `root` is `null`, return it.
2. If the `root` has a `left` child:
    - Set the `left` child's `next` to the `right` child.
    - If the `root` has a `next` pointer, set the `right` child's `next` to the `next` node's `left` child.
    - Recursively connect the `left` subtree.
    - Recursively connect the `right` subtree.
3. Return the `root`.

### CODE

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* left;
    Node* right;
    Node* next;

    Node() : val(0), left(NULL), right(NULL), next(NULL) {}

    Node(int _val) : val(_val), left(NULL), right(NULL), next(NULL) {}

    Node(int _val, Node* _left, Node* _right, Node* _next)
        : val(_val), left(_left), right(_right), next(_next) {}
};
*/

class Solution {
public:
    Node* connect(Node* root) {
        if (!root) return root;

        if (root->left) {
            root->left->next = root->right;
            if (root->next) {
                root->right->next = root->next->left;
            }

            connect(root->left);
            connect(root->right);
        }

        return root;
    }
};
```