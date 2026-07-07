# [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

Given the `head` of a singly linked list, return _the middle node of the linked list_.

If there are two middle nodes, return **the second middle** node.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [3,4,5]
**Explanation:** The middle node of the list is node 3.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/07/23/lc-midlist2.jpg)

**Input:** head = [1,2,3,4,5,6]
**Output:** [4,5,6]
**Explanation:** Since the list has two middle nodes with values 3 and 4, we return the second one.

**Constraints:**

- The number of nodes in the list is in the range `[1, 100]`.
- `1 <= Node.val <= 100`

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
        
    }
}
```

# Solution:

Two Approaches:

## (1) Good Developer Approach:

Calculate length and then Go to Middle (len/2)

len: Length of Linked List

Go to N/2 (Even) or (N+1)/2 (Odd), Print that Node

TC: O(N) + O(N/2) = O(1.5*N)


## (2) Very Good Developer Approach:

Single Traversal: O(N)

Two Pointer/ Hare and Tortoise/ Fast and Slow Pointer

### Approach:

Initial:
	slow = head
	fast = head

During Traversal:
	slow = slow.next
	fast = fast.next.next

When the fast reached End,
	fast.next == None || fast.next.next == None
	Slow ----> Middle of Linked List

### DRY RUN:

```
head: 10 -> 20 -> 30 -> 40 -> 50 -> null

len = 5
Middle: 30 : OP


head = 10

Initial:

slow = 10
fast = 10


During Traversal:

slow = 10 -> 20
fast = 10 -> 20 -> 30


slow = 20 -> 30: Middle of Linked List
fast = 30 -> 40 -> 50: END

OP: 30
```

### CODE:

```java
class Solution {
    public ListNode middleNode(ListNode head) 
    {
        ListNode slow = head;
        ListNode fast = head;

        // Contains 0 or 1 Number of Nodes
        if (head == null || head.next == null)
            return head;

        // First Middle Node
        // while(fast.next!=null && fast.next.next!=null)
        // Second Middle Node
        while(fast!=null && fast.next!=null)
        {
            slow = slow.next; //x , TC: O(1)
            fast = fast.next.next; // 2x, TC: O(1)
        }

        return slow;
    }
```

TC: O(N/2)
SC: O(1)

