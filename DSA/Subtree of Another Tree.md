# [572. Subtree of Another Tree](https://leetcode.com/problems/subtree-of-another-tree/)

Given the roots of two binary trees `root` and `subRoot`, return `true` if there is a subtree of `root` with the same structure and node values of `subRoot` and `false` otherwise.

A subtree of a binary tree `tree` is a tree that consists of a node in `tree` and all of this node's descendants. The tree `tree` could also be considered as a subtree of itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree1-tree.jpg)

**Input:** root = [3,4,5,1,2], subRoot = [4,1,2]
**Output:** true

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/04/28/subtree2-tree.jpg)

**Input:** root = [3,4,5,1,2,null,null,null,null,0], subRoot = [4,1,2]
**Output:** false

**Constraints:**

- The number of nodes in the `root` tree is in the range `[1, 2000]`.
- The number of nodes in the `subRoot` tree is in the range `[1, 1000]`.
- `-104 <= root.val <= 104`
- `-104 <= subRoot.val <= 104`

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
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        
    }
};
```

# Solution 1

## Intuition  

A subtree must match the **entire structure and values** of a portion of the main tree.  

To determine whether `subRoot` is a subtree of `root`, we perform two tasks:  
  
1. Traverse every node in the main tree using DFS.  
2. Whenever a node has the same value as `subRoot`, check whether the two trees rooted at those nodes are **identical**.  

If an exact match is found, we return `true`. Otherwise, we continue searching in the left and right subtrees.  
  
The helper function `isSame()` is responsible for checking whether two trees are exactly the same.  

---  
## Algorithm  

### `isSubtree(root, subRoot)`  
  
1. If `root` is `nullptr`, return whether `subRoot` is also `nullptr`.  
2. If `subRoot` is `nullptr`, return whether `root` is also `nullptr`.  
3. If the current nodes have the same value:  
	- Use `isSame()` to verify that both left and right subtrees are identical.  
	- If they are, return `true`.  
4. Otherwise, recursively search:  
	- the left subtree of `root`  
	- the right subtree of `root`  
5. Return `true` if either recursive call finds a matching subtree.  
---  
### `isSame(root, subRoot)`  
  
1. If both nodes are `nullptr`, the trees match.  
2. If exactly one node is `nullptr`, they do not match.  
3. If the node values differ, return `false`.  
4. Recursively compare:  
	- left children  
	- right children  
5. Return `true` only if both subtrees are identical.  
---  
## Dry Run  

Main tree:  
  
```  
3  
/ \  
4 5  
/ \  
1 2  
```  

Subtree:  

```  
4  
/ \  
1 2  
```  
  
---  
### Start at root (3)  
  
```  
3 != 4  
```  
  
Not a match.  
  
Search left and right.  
  
---  
### Visit node (4)  
  
```  
4 == 4  
```  
  
Call `isSame()`.  
  
Compare:  
  
```  
4 4  
/ \ vs / \  
1 2 1 2  
```  
  
Both roots match.  
  
Left children:  
  
```  
1 == 1  
```  
  
Right children:  
  
```  
2 == 2  
```  
  
All recursive comparisons succeed.  
  
Return:  
  
```  
true  
```  
  
So `isSubtree()` also returns `true`.
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
    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        if(root==nullptr) return subRoot==nullptr;
        if(subRoot==nullptr) return root==nullptr;
        if(root->val==subRoot->val&&isSame(root->left,subRoot->left)&&isSame(root->right,subRoot->right)){
            return true;
        }
        return isSubtree(root->left,subRoot)||isSubtree(root->right,subRoot);
    }
    bool isSame(TreeNode* root, TreeNode* subRoot){
        if(root==nullptr) return subRoot==nullptr;
        if(subRoot==nullptr) return root==nullptr;
        if(root->val==subRoot->val&&isSame(root->left,subRoot->left)&&isSame(root->right,subRoot->right)){
            return true;
        }
        return false;
    }
};
```
### Time & Space Complexity
- Time complexity: O(m∗n)
- Space complexity: O(m+n)

> Where mm is the number of nodes in subRootsubRoot and nn is the number of nodes in rootroot.

# Solution 2: Serialization And Pattern Matching
### Intuition
Instead of comparing trees directly, we can first turn each tree into a **string** and then just check whether one string is contained in the other.

1. **Serialize** both `root` and `subRoot` into strings using the same traversal (for example, preorder).
2. While serializing, we **must include markers for `null` children** (like `#`) and separators (like `$`) so different shapes don't accidentally look the same in the string.
3. Once we have:
    - `S_root` = serialization of the main tree
    - `S_sub` = serialization of the subtree  
        the problem becomes:  
        **"Is `S_sub` a substring of `S_root`?"**

To efficiently check this, we can use a linear-time pattern matching algorithm (like **Z-function** or **KMP**) instead of naive substring search.
### Algorithm
1. **Serialize a tree**
    
    - Use preorder traversal.
    - For each node:
        - Append a separator (e.g., `$`) + node value.
    - For each `null` child, append a special marker (e.g., `#$` or just `#`).
    - This ensures structure and values are uniquely encoded.
2. **Build strings**
    
    - Let `S_sub` = serialization of `subRoot`.
    - Let `S_root` = serialization of `root`.
3. **Combine for pattern matching**
    
    - Build a combined string:  
        `combined = S_sub + "|" + S_root`  
        (`|` is just a separator that does not appear in the serializations.)
4. **Run pattern matching**
    
    - Compute the Z-array (or use another pattern-matching algorithm) on `combined`.
    - Let `m = length(S_sub)`.
    - Scan the Z-values:
        - If any `Z[i] == m`, then `S_sub` appears completely starting at that position → `subRoot` is a subtree → return `true`.
5. If no such position is found, return `false`.
## CODE

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
    string serialize(TreeNode* root) {
        if (root == nullptr) {
            return "$#";
        }
        return "$" + to_string(root->val) +
                serialize(root->left) + serialize(root->right);
    }

    vector<int> z_function(string s) {
        vector<int> z(s.length());
        int l = 0, r = 0, n = s.length();
        for (int i = 1; i < n; i++) {
            if (i <= r) {
                z[i] = min(r - i + 1, z[i - l]);
            }
            while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
                z[i]++;
            }
            if (i + z[i] - 1 > r) {
                l = i;
                r = i + z[i] - 1;
            }
        }
        return z;
    }

    bool isSubtree(TreeNode* root, TreeNode* subRoot) {
        string serialized_root = serialize(root);
        string serialized_subRoot = serialize(subRoot);
        string combined = serialized_subRoot + "|" + serialized_root;

        vector<int> z_values = z_function(combined);
        int sub_len = serialized_subRoot.length();

        for (int i = sub_len + 1; i < combined.length(); i++) {
            if (z_values[i] == sub_len) {
                return true;
            }
        }
        return false;
    }
};
```
### Time & Space Complexity
- Time complexity: O(m+n)
- Space complexity: O(m+n)

> Where mm is the number of nodes in subRootsubRoot and nn is the number of nodes in rootroot.