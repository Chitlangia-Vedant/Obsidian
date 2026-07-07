# [148. Sort List](https://leetcode.com/problems/sort-list/)

Given the `head` of a linked list, return _the list after sorting it in **ascending order**_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

**Input:** head = [4,2,1,3]
**Output:** [1,2,3,4]

**Example 2:**

![](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

**Input:** head = [-1,5,3,4,0]
**Output:** [-1,0,3,4,5]

**Example 3:**

**Input:** head = []
**Output:** []

**Constraints:**

- The number of nodes in the list is in the range `[0, 5 * 104]`.
- `-105 <= Node.val <= 105`

**Follow up:** Can you sort the linked list in `O(n logn)` time and `O(1)` memory (i.e. constant space)?

# Solution : Merge Sort

Uses [[Merge Two Sorted Lists]]

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
    ListNode* sortList(ListNode* start){
        if (start == NULL)
        return nullptr;
        if (start->next == NULL) {
            return start;
        }
        ListNode* fast=start;
        ListNode* slow=start;
        while(fast->next!=NULL&&fast->next->next!=NULL){
            fast=fast->next->next;
            slow=slow->next;
        }
        fast=slow->next;
        slow->next=NULL;
        ListNode* list1=sortList(start);
        ListNode* list2=sortList(fast);
        
        // From Merge Two Sorted Lists
        ListNode dummy(0);
        ListNode* current = &dummy;

        while (list1 != NULL && list2 != NULL) {
            if (list1->val < list2->val) {
                current->next = list1;
                list1 = list1->next;
            } else {
                current->next = list2;
                list2 = list2->next;
            }

            current = current->next;
        }

        // Attach the remaining nodes
        if (list1 != NULL) {
            current->next = list1;
        }else{
            current->next = list2;
        }

        return dummy.next;
    }
};
```



