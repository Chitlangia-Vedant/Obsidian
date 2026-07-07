# [61. Rotate List](https://leetcode.com/problems/rotate-list/)

Given the `head` of a linked list, rotate the list to the right by `k` places.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/11/13/rotate1.jpg)

**Input:** head = [1,2,3,4,5], k = 2
**Output:** [4,5,1,2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/11/13/roate2.jpg)

**Input:** head = [0,1,2], k = 4
**Output:** [2,0,1]

**Constraints:**

- The number of nodes in the list is in the range `[0, 500]`.
- `-100 <= Node.val <= 100`
- `0 <= k <= 2 * 109`

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
    public ListNode rotateRight(ListNode head, int k) {
        
    }
}
```

# Solution:

Pattern/Template: Both Right and Left Rotate the Linked List

(1) K > N, K%= N (Effective Number of Rotations)

Right Shifting an Array:

Optimised Approach: 3 Reversals
- reverse(arr:begin, end)
- reverse(0:k)
- reverse(k:n)


Left Shifting an Array:

Optimised Approach: 3 Reversals
- reverse(arr:begin, end)
- reverse(k:n)
- reverse(0:k)

arr\[(i+k)%n] -----> Not Possible in a Linked List

Right/Left Shift a Linked List:

- Require to go back
- Single LL: There is no way to go back
- Fix: Make LL Circular: tail.next = head, I can travel forward and can connect with previous nodes

## Right Shifting a Linked List:
- Make LL Circular: tail.next = head
- Move to (N-K)th Node
- Link to NULL, (N-k-1).next = NULL
- (N-K)th Node ----> head
## Left Shifting a Linked List:
- Make LL Circular: tail.next = head
- Move to Kth Node
- Link to NULL, (K-1).next = NULL
- Kth Node ----> head

## Right Rotate:

```
head: 1 -> 2 -> 3 -> 4 -> 5 -> NULL

K = 2
N = 5

OP: 4 -> 5 -> 1 -> 2 -> 3 -> NULL


Right Shifting a Linked List:

- Make LL Circular: tail.next = head
- Move to (N-K)th Node
- Link to NULL, (N-k-1).next = NULL
- (N-K)th Node ----> head


- Make LL Circular: tail.next = head
 5.next = 1

- Move to (N-K)th Node
N-K = 5-2 = 3rd Node


- Link to NULL, (N-k-1).next = NULL
1 -> 2 -> 3 -> NULL


At This Moment:

4 -> 5 -> 1 -> 2 -> 3 -> NULL
		 head

I need to bring head to "4"		 

- (N-K)th Node ----> head
HEAD: 4 -> 5


Final LL:

head: 4 -> 5 -> 1 -> 2 -> 3 -> NULL
```

# CODE:

```Java
class Solution {
    public ListNode rotateRight(ListNode head, int K) 
    {
        // Check constraints, can be null 
        if (head == null)
            return head;

        int len = 1;
        ListNode tail = head;

        // Find tail(last node) and calculate length
        while(tail.next!=null)
        {
            len++;
            tail = tail.next;
        }

        // tail -> 5
        // LL: [1 2 3 4 5], K = 2, len = 5

        // Make LL Circular
        tail.next = head; // [1 2 3 4 5->1]
        K = K % len; // k can be greater than n // K = 2 % 5 = 2

        // tail: 5
        // Move to (N-K)th Node
        // len - K = 5-2 = 3
        for (int i=0; i<len - K; i++) // i=0,1,2
        {
            tail = tail.next;
            // i = 0, tail : 5 -> 1
            // i = 1, tail : 1 -> 2
            // i = 2, tail : 2 -> 3
        }
        // tail: 3, tail.next = 4

        head = tail.next; // head: 4
        tail.next = null; // Circular LL -> Single LL -> 3 -> null

        return head; // 4 -> 5 -> 1 -> 2 -> 3 -> NULL
    }
}
```


TC: O(N)
SC: O(1)