  # [92. Reverse Linked List II](https://leetcode.com/problems/reverse-linked-list-ii/)

Given the `head` of a singly linked list and two integers `left` and `right` where `left <= right`, reverse the nodes of the list from position `left` to position `right`, and return _the reversed list_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

**Input:** head = [1,2,3,4,5], left = 2, right = 4
**Output:** [1,4,3,2,5]

**Example 2:**

**Input:** head = [5], left = 1, right = 1
**Output:** [5]

**Constraints:**

- The number of nodes in the list is `n`.
- `1 <= n <= 500`
- `-500 <= Node.val <= 500`
- `1 <= left <= right <= n`

**Follow up:** Could you do it in one pass?

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
    public ListNode reverseBetween(ListNode head, int left, int right) {
        
    }
}
```

# Solution:

```
Values   2 3 4 5 6
Pos      1 2 3 4 5

left, right -----> positions (1 based)

left = 2
right = 5

OP:
[2 6 5 4 3]

(1) Changes: Reverse: [left, right]
(2) No Change in [head, left-1] and [right+1, tail]
```
## Approach:

(1) Reach to left node
(2) Apply Reversal Logic from [left, right]
## CODE:

// TC: O(left) + O(right-left)
// SC: O(1)

```Java
class Solution 
{
    public ListNode reverseBetween(ListNode head, int left, int right) 
    {
        ListNode dummy = new ListNode(-1);
        dummy.next = head; // dummy: -1 -> 1 -> 2 -> 3 -> 4 -> 5
        ListNode prev = dummy; // prev = -1 -> 1 -> 2 -> 3 -> 4 -> 5
        ListNode curr = dummy.next; // curr = 1 -> 2 -> 3 -> 4 -> 5

        int i = 1;
        while (i < left) // i=1 , i<2
        {
            prev = curr; // prev = 1 -> 2 -> 3 -> 4 -> 5
            curr = curr.next; // curr = 2 -> 3 -> 4 -> 5
            i++;
        }

        ListNode res = prev; // res = 1 -> 2 -> 3 -> 4 -> 5
        while (i<=right) // i=2, i<4
        {
            ListNode temp = curr.next;
            curr.next = prev;
            prev = curr;
            curr = temp;
            i++;
        }

/*
         i = 2
         temp = 3 -> 4 -> 5
         Orig curr = 2 -> 3 -> 4 -> 5
         curr.next = prev
         curr = 2 -> 1 -> 2 -> 3 -> 4 -> 5
         prev = 2 -> 1 -> 2 -> 3 -> 4 -> 5
         curr = 3 -> 4 -> 5

         i = 3
         temp = 4 -> 5
         Orig curr = 3 -> 4 -> 5
         curr.next = prev
         curr = 3 -> 2 -> 1 -> 2 -> 3 -> 4 -> 5
         prev = 3 -> 2 -> 1 -> 2 -> 3 -> 4 -> 5
         curr = 4 -> 5

         i = 4
         temp = 5 -> null
         Orig curr = 4 -> 5
         curr.next = prev
         curr = 4 -> 3 -> 2 -> 1 -> 2 -> 3 -> 4 -> 5
         prev = 4 -> 3 -> 2 -> 1 -> 2 -> 3 -> 4 -> 5
         curr = 5 -> null


        Orig res = 1 -> 2 -> 3 -> 4 -> 5
*/

        res.next.next = curr; 
        // res = 1 -> 2 -> 5 -> null

        //prev = 4 -> 3 -> 2 -> 1 -> 2 -> 3 -> 4 -> 5
        // 2.next = 5 in res

        res.next = prev;
        // res = 1 -> 4 -> 3 -> 2 -> 5 (Since 2.next = 5)

        return dummy.next;
        // -1 -> [1 -> 4 -> 3 -> 2 -> 5]

    }

}
```