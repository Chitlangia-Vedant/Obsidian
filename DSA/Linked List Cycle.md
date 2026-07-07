# [141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

Given `head`, the head of a linked list, determine if the linked list has a cycle in it.

There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to. **Note that `pos` is not passed as a parameter**.

Return `true` _if there is a cycle in the linked list_. Otherwise, return `false`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

**Input:** head = [3,2,0,-4], pos = 1
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 1st node (0-indexed).

**Example 2:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test2.png)

**Input:** head = [1,2], pos = 0
**Output:** true
**Explanation:** There is a cycle in the linked list, where the tail connects to the 0th node.

**Example 3:**

![](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist_test3.png)

**Input:** head = [1], pos = -1
**Output:** false
**Explanation:** There is no cycle in the linked list.

**Constraints:**

- The number of the nodes in the list is in the range `[0, 104]`.
- `-105 <= Node.val <= 105`
- `pos` is `-1` or a **valid index** in the linked-list.

**Follow up:** Can you solve it using `O(1)` (i.e. constant) memory?

```Java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        
    }
}
```

# Understanding:

Cycle/Loop in a Linked List:

If a "Node" is Traversed MORE THAN ONCE During Traversal
Then Its a Loop/Cycle in a Linked List

Case-1:

```
head
 1 -----> 2 -----> 3 ------> 4
		 /|\				 |
		  |					 |
		  |					 |
		  |__________________|

Traversal OP:
1 2 3 4 2 3 4 2 3 4 2 3 4.......Infinite

Cycle: YES
Cycle at Middle
```

Case-2:

```
head
 1 -----> 2 -----> 3 ------> 4
 /|\						 |
  |							 |
  |							 |
  |__________________________|


Traversal OP:
1 2 3 4 1 2 3 4 1 2 3 4 1 2 3 4......Infinite

Cycle: YES
Cycle at Beginning
```

Case-3:

```
head
 1 -----> 2 -----> 3 ------> 4--------
							/|\       |
							 |________|



Traversal OP:
1 2 3 4 4 4 4 4 4 4......Infinite

Cycle: YES
Cycle at Vertex (End Node)
```

Case-4:

```
head
 1 -----> 2 -----> 3 ------> 4

Traversal OP:
1 2 3 4

Cycle: NO
```

Case-5: 

```
head
 2 -----> 2 -----> 2 ------> 2

Traversal OP:
2 2 2 2

Cycle: NO
```

**Note:** 
	For Cycle, "Nodes" should be Repeated, Not the Value

# Solution:

Follow up: Can you solve it using O(1) (i.e. constant) memory?

TC: ?
SC: O(1)

## (1) With Hashing

Intuition:
Same Node is traversed more than once in a LL - Cycle
Else, No Cycle

Set:
Check if a Node is Already present or not

Approach:
Put Nodes in a Set and check

NOTE:
DO NOT PUT NODE.DATA IN SET
(Multiple Nodes can contain the same value)
### CODE:

```Java
// Approach-1: With Hashing
public class Solution {
    public boolean hasCycle(ListNode head) 
    {
        // Constraints:
        //The number of the nodes in the list is in the range [0, 104].
        if (head == null)
            return false;

// Check for Duplication of NODE, NOT for Duplication of Data in Node
        HashSet<ListNode> set = new HashSet<ListNode>();
        ListNode temp = head;

        while (temp!=null)
        {
        // Node Already Contained -----> Cycle
            if (set.contains(temp))
                return true;

            set.add(temp);
            temp = temp.next;
        }

        return false; // No Cycle
    }
}
```

TC: O(N)
SC: O(N) - HashSet

## (2) Space Optimised Approach:

Approach:
- Floyd Cycle Detection Algorithm

(1) slow: head, fast: head
(2) slow: Speed -> x, fast: Speed -> 2x/3x/4x/5x
(3) if slow == fast: CYCLE
(4) Else, No Cycle

TC: O(N/2)
SC: O(1)

### CODE:

```Java
// Approach-2: Without Hashing

public class Solution {
    public boolean hasCycle(ListNode head) 
    {
        // Constraints:
        //The number of the nodes in the list is in the range [0, 104].
        if (head == null)
            return false;

        ListNode slow = head;
        ListNode fast = head;

        // slow: x, fast: 2x
        while(fast.next!=null && fast.next.next!=null)
        {
            slow = slow.next; // speed: x
            fast = fast.next.next; // speed: 2x
            
            if (slow == fast)
                return true;
        }

        return false;
    }
}
```

TC: O(N/2)
SC: O(1)
