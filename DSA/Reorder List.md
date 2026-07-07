# [143. Reorder List](https://leetcode.com/problems/reorder-list/)

You are given the head of a singly linked-list. The list can be represented as:

L0 → L1 → … → Ln - 1 → Ln

_Reorder the list to be on the following form:_

L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …

You may not modify the values in the list's nodes. Only nodes themselves may be changed.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/03/04/reorder1linked-list.jpg)

**Input:** head = [1,2,3,4]
**Output:** [1,4,2,3]

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/03/09/reorder2-linked-list.jpg)

**Input:** head = [1,2,3,4,5]
**Output:** [1,5,2,4,3]

**Constraints:**

- The number of nodes in the list is in the range `[1, 5 * 104]`.
- `1 <= Node.val <= 1000`

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
    void reorderList(ListNode* head) {
        
    }
};
```

# Solution 1: Brute-Force Algorithm

1. First some base cases that we need to take care i.e if the linked list has zero,one or two elements just return it
2. Now next we need to find the penultimate node, so after finding it we can start the relinking process
3. Now start the relinking process as 1st node with last node ,2nd node with penultimate node, 3rd node with 3rd last node ......
4. Now repeat 2nd and 3rd steps

## CODE

```cpp
//Upvote  and Comment
class Solution {
public:
    void reorderList(ListNode* head) {
        //base case i.e if the linked list has zero,one or two elments just return it
        if(!head || !head->next || !head->next->next) return;
        
        //Find the penultimate node i.e second last node of the linkedlist
        ListNode* penultimate = head;
        while (penultimate->next->next) penultimate = penultimate->next;
        
        // Link the penultimate with the second element
        penultimate->next->next = head->next;
        head->next = penultimate->next;
        
        //Again set the penultimate to the the last 
        penultimate->next = NULL;
        
        // Do the above steps rcursive
        reorderList(head->next->next);
    }
};
```
# Solution 2: Vector

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
    void reorderList(ListNode* head) {
        vector<ListNode*> vec;
        while(head!=NULL){
            vec.push_back(head);
            head=head->next;
        }
        head=vec[0];
        
        for(int i=0;i<vec.size()/2;i++){
            vec[i]->next=vec[vec.size()-1-i];
            vec[vec.size()-1-i]->next=vec[i+1];
        }
        vec[vec.size()/2]->next=NULL;
        return;
    }
};
```

# Solution 3: Two pointer Approach \[Tortoise and Hare method]

### Step 1: Splitting the List into Two Halves

Using the **two-pointer technique**, we can identify the middle of the list:

- `slow` moves one step at a time.
- `fast` moves two steps at a time.

When `fast` reaches the end, `slow` will be at the midpoint of the list.

Example:

- Input: `1 → 2 → 3 → 4 → 5`
- After splitting: `1 → 2 → 3` and `4 → 5`

### Step 2: Reversing the Second Half

Next, we reverse the second half of the list. Starting from the node after `slow`, we use a standard iterative reversal process to reverse the pointers.

Example:

- Before reversing: `4 → 5`
- After reversing: `5 → 4`

### Step 3: Merging the Two Halves

Finally, we merge the two halves alternately:

- Take a node from the first half, then a node from the reversed second half.
- Repeat until all nodes are merged.

Example:

- First half: `1 → 2 → 3`
- Reversed second half: `5 → 4`
- Merged: `1 → 5 → 2 → 4 → 3`

## Visual Explanation

### Initial List

```
1 → 2 → 3 → 4 → 5
```

### Step 1: Splitting the List

1. Initialize `slow` and `fast` to the head of the list.
2. Move `slow` one step and `fast` two steps at a time.

```sql
slow: 3
fast: null (end of the list)
```

List is split:

```sql
First half: 1 → 2 → 3
Second half: 4 → 5
```

---

### Step 2: Reversing the Second Half

Using an iterative reversal process:

- Start with `node = null` and `second = 4 → 5`.

1. Reverse 4:

```sql
node: 4 → null
second: 5
```

2. Reverse 5:

```sql
node: 5 → 4 → null
second: null
```

Reversed second half:

```
5 → 4
```

---

### Step 3: Merging the Two Halves

1. Initialize:
    
    - `first = 1 → 2 → 3`
    - `second = 5 → 4`
2. Merge nodes alternately:
    
    - Take `1` from `first` and `5` from `second`:
        
        ```
        1 → 5
        ```
        
    - Take `2` from `first` and `4` from `second`:
        
        ```
        1 → 5 → 2 → 4
        ```
        
    - Take `3` from `first` (second is now null):
        
        ```
        1 → 5 → 2 → 4 → 3
        ```

## CODE

```cpp
class Solution {
public:
    void reorderList(ListNode* head) {
        if (!head) return;

        // Step 1: Find the middle of the list
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }

        // Step 2: Reverse the second half of the list
        ListNode* second = slow->next;
        slow->next = nullptr;
        ListNode* node = nullptr;

        while (second) {
            ListNode* temp = second->next;
            second->next = node;
            node = second;
            second = temp;
        }

        // Step 3: Merge the two halves
        ListNode* first = head;
        second = node;

        while (second) {
            ListNode* temp1 = first->next;
            ListNode* temp2 = second->next;
            first->next = second;
            second->next = temp1;
            first = temp1;
            second = temp2;
        }
    }
};
```

## Complexity Analysis

1. **Time Complexity**: O(n)
    
    - Finding the middle takes O(n).
    - Reversing the second half takes O(n/2).
    - Merging the two halves takes O(n).
2. **Space Complexity**: O(1)
    
    - The algorithm uses constant extra space as it modifies the list in-place.`_