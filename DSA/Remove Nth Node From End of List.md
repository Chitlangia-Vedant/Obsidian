# [19. Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Given the `head` of a linked list, remove the `nth` node from the end of the list and return its head.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

**Input:** head = [1,2,3,4,5], n = 2
**Output:** [1,2,3,5]

**Example 2:**

**Input:** head = [1], n = 1
**Output:** []

**Example 3:**

**Input:** head = [1,2], n = 1
**Output:** [1]

**Constraints:**

- The number of nodes in the list is `sz`.
- `1 <= sz <= 30`
- `0 <= Node.val <= 100`
- `1 <= n <= sz`

**Follow up:** Could you do this in one pass?

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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        
    }
}
```

# Solution:

[[#(2) Very Good Developer Approach|Two Pointer/Hare and Tortoise/Fast and Slow Pointers Approach]]

```Java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode fast = head, slow = head;
        
        for (int i = 0; i < n; i++) // O(K)
            fast = fast.next; 
        
        // When asked to remove last node/first node from end
        if (fast == null) 
            return head.next;
        
        while (fast.next != null) // (N-K)
        {
            fast = fast.next;
            slow = slow.next;
        }
       
        slow.next = slow.next.next; // Removing the Kth Node from End
        return head;
    }
}
```


TC: O(N) \[O(K) + O(N-K)]
SC: O(1)

\[1]
K = 1

#  Kth Node from End in a Linked List: SPECIAL

head: 10 -> 20 -> 30 -> 40 -> 50 -> null

K = 1
OP: 50

K = 4
OP: 20

Constraints:
1 \<= K \<= N

```
int KthNodeFromEnd(Node head, int K)
{

}
```

## Solution:

Two Approaches:

### (1) Good Developer Approach

Kth Node from End
= (len - K + 1) from Beginning

len: Length of LL

K = 1
len = 5

From Beginning = len - K + 1 = 5-1+1 = 5th Node from Beginning

Complexity:

(1) Calculate Length of Linked List: O(N)
(2) Traverse (len - K + 1) from Beginning and then return that Node: O(N-K+1)

TC: O(N) + O(N-K+1) = O(2*N)
SC: O(1)

Worst Case:
K = 1

In Real Life Production:

N = 10^9

O(N): Single Traversal: 10^9 Iterations
O(2*N): Double Traversal: 2*10^9 Iterations


### (2) Very Good Developer Approach

Single Traversal: O(N)

**Two Pointer/Hare and Tortoise/Fast and Slow Pointers Approach**

Initial:
	slow: head
	fast: head + K

Start Traversal:
	(Move Both by 1 Step - Same Speed)
	slow = slow.next
	fast = fast.next

End
	fast == null: STOP
	slow -> Kth Node from End


TC: O(N)
SC: O(1)

#### DRY RUN:

```
head: 10 -> 20 -> 30 -> 40 -> 50 -> null

K = 2
OP: 40
head = 10

Initial:
slow = 10
fast = head + K = 30

Start Traversal:

slow = 10 -> 20
fast = 30 -> 40


slow = 20 -> 30
fast = 40 -> 50


slow = 30 -> 40: Kth NOde from End
fast = 50 -> null: STOP


OP: 40
```

#### CODE:

```
`int KthNodeFromEnd(Node head, int K)
{
	Node slow = head;
	Node fast = head;

	while(K!=0) // O(K)
	{
		fast = fast.next;
		K--;
	}

	// slow -> head, fast = head + k

	while(fast!=null) // O(N-K)
	{
		slow = slow.next;
		fast = fast.next;
	}

	// At This Moment, fast -> null
	// slow -> Kth Node from End
	return slow.data;
}`
```

TC: O(N)
SC: O(1)