# [25. Reverse Nodes in k-Group](https://leetcode.com/problems/reverse-nodes-in-k-group/)

Given the `head` of a linked list, reverse the nodes of the list `k` at a time, and return _the modified list_.

`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes, in the end, should remain as it is.

You may not alter the values in the list's nodes, only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

**Input:** head = [1,2,3,4,5], k = 2
**Output:** [2,1,4,3,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

**Input:** head = [1,2,3,4,5], k = 3
**Output:** [3,2,1,4,5]

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

**Follow-up:** Can you solve the problem in `O(1)` extra memory space?

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
    public ListNode reverseKGroup(ListNode head, int k) {
        
    }
}
```

# Solution:

## (1) Reverse a Complete Linked List


```Java
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
```

## (2) Reverse a Linked List in Size of K:

```Java
    	while(curr!=null && count<K)
    	{
    		// Store the Next Node before changing curr
			next = curr.next; // next = 2, next = 3, next = null

			// Actual Reversal
			curr.next = prev; // 1.next = null, 2.next = 1, 3.next = 2

			// Move prev and curr by 1 Step
			prev = curr; // prev = 1, prev = 2, prev = 3
			curr = next; // curr = 2, curr = 3, curr = null
			count++;
    	}
```


## Approach:

Reverse First K Nodes,
-----> N-K < K, Nothing left to Reverse
-----> Else, Reverse for Pending Nodes in Size of K (Recur/Iterate on same function)

## CODE:

```java
class Solution {
    public ListNode reverseKGroup(ListNode head, int K) 
    {
        if(head == null)
            return head;
        
        ListNode curr = head;
        int count = 0;

        // Reverse first K Nodes of Linked List
        while (curr!=null && count<K) // O(K)
        {
            curr = curr.next;
            count++;
        }

    /*
        After this while loop, curr.next ---> (K+1)th Node
        2 Cases:
        (1) K = N: Nothing left to reverse
        (2) K!= N: Reverse for Pending Node in LL
    */

        if (count == K)
        {
            // Recur starting from (K+1)th Node
            curr = reverseKGroup(curr, K); // O(N-K)

            // head ---> pointer to direct part
            // curr ---> pointer to revrese part
            // count = k: Reversing K Times

            while (count-- > 0) // count = K, initially
            {
                ListNode temp = head.next;
                head.next = curr;
                curr = head;
                head = temp;
            }

            head = curr; // Reverse
        }

    // No Else Case ----> Directly return head ----> Return as it as when window_Size < k -> No reversals
        return head;
    }
}
```


TC: O(N)
SC: O(1) - Extra Space
    O(N) - Recursion Stack/ Auxiliary Space