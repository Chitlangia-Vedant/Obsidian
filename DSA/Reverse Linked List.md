# [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

Given the `head` of a singly linked list, reverse the list, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [5,4,3,2,1]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

**Input:** head = [1,2]
**Output:** [2,1]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

- The number of nodes in the list is the range `[0, 5000]`.
- `-5000 <= Node.val <= 5000`

**Follow up:** A linked list can be reversed either iteratively or recursively. Could you implement both?

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
    public ListNode reverseList(ListNode head) {
        
    }
}
```

# Solution:

(1) Stack Based Solution
(2) Using Three Pointers - Iterative
(3) Using Three Pointers - Recursive

## (1) Stack Based Solution:

- Reverse Nodes + Adjust Pointers

Approach:
- Put List Nodes into Stack

(Why Nodes Not Data ----> Node = data + next)

1 2 3 4 5 ----> 5 4 3 2 1

IP: 1 -> 2 -> 3 -> 4 -> null

OP: 4 -> 3 -> 2 -> 1 -> null

### Steps:

- head: Initial temp
- Start Popping from Stack:
	`tail.next = stack.pop()`
- Last Node (Initial head).next -> null

### CODE:

```Java
class Solution {
    public ListNode reverseList(ListNode head)
    {
        ListNode curr = head;
        Stack<ListNode> stack = new Stack<ListNode>();

        // Travel to Last Node of Linked List
        // After that, head -> Last Node
        while (curr!=null)
        {
            stack.push(curr);
            curr = curr.next;
        }

        ListNode res = new ListNode(-1);
        curr = res; // curr: last node: -1
        // Reverse: -1 -> null

        while(!stack.isEmpty())
        {
            curr.next = stack.pop();
            curr = curr.next;
        }
        // Reverse: -1 -> 5 -> 4 -> 3 -> 2 -> 1

        curr.next = null; // Last Node.next ----> null
        // Reverse: -1 -> 5 -> 4 -> 3 -> 2 -> 1 -> null

        return res.next; // Reversed Linked List
        // Reverse.next:  5 -> 4 -> 3 -> 2 -> 1 -> null
    }
}
```

TC: O(N) + O(N)
SC: O(N) - Stack

## (2) Using 3 Pointers, O(1) Space

Initial:
prev = null
curr = head
next = null

Iterate through LL:

// Store the Next Node before changing curr
next = curr.next;

// Actual Reversal
curr.next = prev;

// Move prev and curr by 1 Step
prev = curr;
curr = next;

### CODE:

```Java
class Solution 
{
    public ListNode reverseList(ListNode head)
    {
    	ListNode prev = null; // prev = null
    	ListNode curr = head; // curr = 1
    	ListNode next = null; // next = null

    	while(curr!=null)
    	{
    		// Store the Next Node before changing curr
			next = curr.next; // next = 2, next = 3, next = null

			// Actual Reversal
			curr.next = prev; // 1.next = null, 2.next = 1, 3.next = 2

			// Move prev and curr by 1 Step
			prev = curr; // prev = 1, prev = 2, prev = 3
			curr = next; // curr = 2, curr = 3, curr = null
    	}

    	After 1st Iteration: curr = 2, next = 2, prev = 1, 1.next = null
    	After 2nd Iteration: curr = 3, next = 3, prev = 2, 2.next = 1
    	After 3rd Iteration: curr = null, next = null, prev = 3, 3.next = 2


    	head = prev; // head: 3 -> 2 -> 1 -> null
    	return head;
    }
}
```

TC: O(N)
SC: O(1)

### DRY RUN:

Orig LL: 1 -> 2 -> 3 -> null
Rev LL: 3 -> 2 -> 1 -> null

head = 1 <- curr
prev = null
next = null