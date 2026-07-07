# [445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contains a single digit. Add the two numbers and return the sum as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/04/09/sumii-linked-list.jpg)

**Input:** l1 = [7,2,4,3], l2 = [5,6,4]
**Output:** [7,8,0,7]

**Example 2:**

**Input:** l1 = [2,4,3], l2 = [5,6,4]
**Output:** [8,0,7]

**Example 3:**

**Input:** l1 = [0], l2 = [0]
**Output:** [0]

**Constraints:**

- The number of nodes in each linked list is in the range `[1, 100]`.
- `0 <= Node.val <= 9`
- It is guaranteed that the list represents a number that does not have leading zeros.

**Follow up:** Could you solve it without reversing the input lists?

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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
    }
};
```
# Solution
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
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        stack<int> i,j;
        ListNode *head=l1, *temp=l1;
        while(l1!=NULL){
            i.push(l1->val);
            l1=l1->next;
        }
        while(l2!=NULL){
            j.push(l2->val);
            l2=l2->next;
        }
        stack<int> s;
        int carry=0;
        while((!i.empty())||(!j.empty())){
            int a=0;
            if(!i.empty()){
                a=i.top();
                i.pop();
            }
            int b=0;
            if(!j.empty()){
                b=j.top();
                j.pop();
            }
            s.push((a+b+carry)%10);
            carry=(a+b+carry)/10;
        }
        if(carry!=0){
            s.push(carry);
        }
        while(!s.empty()){
            temp->val=s.top();
            s.pop();
            if(temp->next==NULL&&!s.empty()){
                temp->next=new ListNode(0);
            }
            temp=temp->next;
        }
        return head;
    }
};
```

# Solution 2

1. The `rev` function is defined to reverse a linked list iteratively. It uses three pointers: `curr`, `prev`, and `nex`. It starts with `curr` pointing to the head of the list. It iteratively updates the `next` pointer of each node to point to the previous node. After updating the pointers, `prev` is updated to `curr`, `curr` is updated to `nex`, and `nex` is updated to `curr->next`. The function returns `prev`, which becomes the new head of the reversed list.
2. The `rev2` function is defined to reverse a linked list recursively using the divide-and-conquer approach. It checks the base cases: if the `head` is `NULL`, it returns `NULL`, and if the `head->next` is `NULL`, it returns `head` itself. Otherwise, it recursively calls `rev2` on `head->next` to reverse the remaining sublist. After getting the reversed sublist, it updates the `next` pointer of the second node to point to the `head`, sets `head->next` to `NULL`, and returns the new head of the reversed list.

You can use any method recursive or inerative to reverse a List.

3. The `solve` function performs the addition of two reversed linked lists. It initializes a carry variable as 0 and creates a dummy node `p` to store the result. It iterates over both linked lists simultaneously, adding the corresponding digits along with the carry. It calculates the sum, updates the carry, creates a new node with the value of the sum % 10, and appends it to the result linked list. The iteration continues until both input lists and the carry are exhausted.
4. The `addTwoNumbers` function is the main entry point of the code. It first reverses the first input linked list using the `rev` function and reverses the second input linked list using the `rev2` function. Then, it calls the `solve` function with the reversed lists as arguments to perform the addition. The result is a reversed linked list, so it reverses the result again using the `rev` function before returning it.

## Complexity
- Time complexity:O(n)
- Space complexity:O(1)

## Code

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
  ListNode* rev(ListNode* head) {
          if(head==NULL)
            return NULL;
    ListNode*curr=head;
    ListNode*prev=NULL;
    ListNode*nex=NULL;
        while(curr!=NULL){
            nex=curr->next;
            curr->next=prev;
            prev=curr;
            curr=nex;
        }
        return prev;
    }
    ListNode*rev2(ListNode*head){
        if(head==NULL){
            return NULL;
        }
        if(head->next==NULL){
            return head;
        }
        ListNode*newhead=rev2(head->next);
        head->next->next=head;
        head->next=NULL;
        return newhead;
    }
    ListNode* solve(ListNode* head1, ListNode* head2) {
          int car=0;
    ListNode*p=new ListNode(9);
        ListNode*ans=p;
    while(head1!=NULL||head2!=NULL||car>0){
        int sum=car;
        if(head1!=NULL){
            sum+=head1->val;
            head1=head1->next;
        }
        if(head2!=NULL){
            sum+=head2->val;
            head2=head2->next;
        }
        int v=sum%10;
        car=sum/10;
        ans->next=new ListNode(v);
        ans=ans->next;
    }
    return p->next;;
    }
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode*l1rev=rev(l1);
        ListNode*l2rev=rev2(l2);
        ListNode*res=solve(l1rev,l2rev);
        return rev(res);
    }
};
```