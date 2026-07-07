# [103. Binary Tree Zigzag Level Order Traversal](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

Given the `root` of a binary tree, return _the zigzag level order traversal of its nodes' values_. (i.e., from left to right, then right to left for the next level and alternate between).

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
**Input:** root = [3,9,20,null,null,15,7]
**Output:** [[3],[20,9],[15,7]]
```

**Example 2:**

```
**Input:** root = [1]
**Output:** [[1]]
```

**Example 3:**

```
**Input:** root = []
**Output:** []
```

**Constraints:**

- The number of nodes in the tree is in the range `[0, 2000]`.
- `-100 <= Node.val <= 100`

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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        
    }
}
```

# Solution:

OP:

List of Lists/Vector of Vectors/2D Array

Each Level: [List]: L to R, R to L, based upon level
Final Ans: [List of Lists]

Ways:

## (1) Generic Solution: All Languages
```
count % 2 == 0: L to R
	Else R to L
```
count: level

## (2) Python Specific Solution

```
arr = [1 2 3 4]
print(arr[::1])
```

OP: 

```
arr = [1, 2, 3, 4]
print(arr[::1]) // [1, 2, 3, 4]
```


```
print(arr[::-1]) // [4, 3, 2, 1]


-1 * -1 * -1 * -1 * -1............... Even Times = 1
									  Odd Times = -1
```


```
arr[::1]: L to R
arr[::-1]: R to L
```

After Every Level, Multiply by -1

## CODE:

``` Python
from collections import deque

class Solution(object):
    def zigzagLevelOrder(self, root):
        
        if not root:
            return []
        
        queue = deque([root])
        result, direction = [], 1 # result: 2-D List
        
        while queue:
            level = [] # 1-D List
            
            for i in range((len(queue))):
                node = queue.popleft()
                level.append(node.val)
                
                 # Going from left to right at any level
                if node.left: 
                    queue.append(node.left)
                    
                if node.right: 
                    queue.append(node.right)
                    
            result.append(level[::direction])
            
            direction *= (-1) # Comment this line -> Will become LOT/BFS
            # 1 * -1 = -1: right to left, 
            # -1 * -1 = 1: left to right - Alternate
        
        return result    
            
```

TC: O(N)
SC: O(N) - Queue

# Solution-2:

```Java
// Approach - Using BFS/LOT - Go to each level and figure L to R or R to L based upon count of each level

class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) 
    {
        List<List<Integer>> answer = new ArrayList<>();
        
        if (root == null)
            return answer;
        
        int level = 0;
        bfsHelper(root, answer, level);
        
        return answer;
    }
    
    // ZigZag BFS/LOT
    public void bfsHelper(TreeNode root, List<List<Integer>> answer, int level)
    {
        Queue<TreeNode> q = new LinkedList<>();
        q.offer(root);
        
        while (!q.isEmpty())
        {
            int size = q.size();
            
            // Results of Nodes at 1 Particular Level
            
            List<Integer> levelNodeList = new ArrayList<>();
            int i = 0;
            
            for (i=0; i<size; i++)
            {
                TreeNode temp = q.poll();
                levelNodeList.add(temp.val);
                
                // Recur on Left Subtree
                if (temp.left!=null)
                    q.add(temp.left);
                
                
                // Recur on Right Subtree
                if (temp.right!=null)
                    q.add(temp.right);                
            }
            
            // After this for loop, 
            // I have LEFT TO RIGHT Values of a A Particular Level 
            
            // Based upon ODD/EVEN, Change the Direction (Reverse)
            
            // for odd level, reverse the levelNodeList            
            if (level % 2 != 0)
                Collections.reverse(levelNodeList);
            // [9 20 ] -> [20 9]
            
            level++;
            
          answer.add(levelNodeList);                        
        }        
    }
}
```

TC - O(N) 
SC - O(N) - Queue