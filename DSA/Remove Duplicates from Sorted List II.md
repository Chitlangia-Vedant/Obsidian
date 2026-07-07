# [82. Remove Duplicates from Sorted List II](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

Given the `head` of a sorted linked list, _delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list_. Return _the linked list **sorted** as well_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

**Input:** head = [1,2,3,3,4,4,5]
**Output:** [1,2,5]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

**Input:** head = [1,1,1,2,3]
**Output:** [2,3]

**Constraints:**

- The number of nodes in the list is in the range `[0, 300]`.
- `-100 <= Node.val <= 100`
- The list is guaranteed to be **sorted** in ascending order.

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        
    }
};
```

# Solution 1

## CODE (Mine)

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        unordered_map<int,int> mp;
        ListNode* temp=head;
        while(temp!=NULL){
            mp[temp->val]++;
            temp=temp->next;
        }
        ListNode dummy(0,head);
        temp=&dummy;
        while(head!=NULL){
            if(mp[head->val]>1){
                temp->next=head->next;
            }else{
                temp=head;
            }
            head=head->next;
        }
        return dummy.next;
    }
};
```

# Solution 2

- Use a dummy node pointing to the head.
- Traverse the list using two pointers: `prev` (last confirmed unique node) and `cur` (current node being inspected).
- If `cur.val == cur.next.val`, skip all nodes with the same value.
- Otherwise, move `prev` forward.
- Return the list starting from `dummy.next`.

## Complexity

- **Time complexity:** O(n)
- **Space complexity:** O(1)

## CODE

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) return head;

        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* prev = dummy;
        ListNode* cur = head;

        while (cur && cur->next) {
            if (cur->val == cur->next->val) {
                while (cur->next && cur->val == cur->next->val) {
                    cur = cur->next;
                }
                prev->next = cur->next; // Skip all duplicates
            } else {
                prev = prev->next; // Move to next unique node
            }
            cur = cur->next;
        }

        return dummy->next;
    }
};
```