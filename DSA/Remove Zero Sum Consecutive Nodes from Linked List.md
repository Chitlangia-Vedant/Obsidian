# [1171. Remove Zero Sum Consecutive Nodes from Linked List](https://leetcode.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)

Given the `head` of a linked list, we repeatedly delete consecutive sequences of nodes that sum to `0` until there are no such sequences.

After doing so, return the head of the final linked list.  You may return any such answer.

(Note that in the examples below, all sequences are serializations of `ListNode` objects.)

**Example 1:**

**Input:** head = [1,2,-3,3,1]
**Output:** [3,1]
**Note:** The answer [1,2,1] would also be accepted.

**Example 2:**

**Input:** head = [1,2,3,-3,4]
**Output:** [1,2,4]

**Example 3:**

**Input:** head = [1,2,3,-3,-2]
**Output:** [1]

**Constraints:**

- The given linked list will contain between `1` and `1000` nodes.
- Each node in the linked list has `-1000 <= node.val <= 1000`.

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
    ListNode* removeZeroSumSublists(ListNode* head) {
        
    }
};
```

# Solution: Prefix sum

Iterate for the first time,  
calculate the prefix sum,  
and save the it to `mp[sum]`

Iterate for the second time,  
calculate the prefix sum,  
and directly skip to last occurrence of this prefix sum.

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
    ListNode* removeZeroSumSublists(ListNode* head) {
        ListNode dummy(0, head);
        unordered_map<int, ListNode*> mp;

        int sum = 0;
        for (ListNode* cur = &dummy; cur; cur = cur->next) {
            sum += cur->val;
            mp[sum] = cur;          // last occurrence
        }

        sum = 0;
        for (ListNode* cur = &dummy; cur; cur = cur->next) {
            sum += cur->val;
            cur->next = mp[sum]->next;
        }

        return dummy.next;
    }
};
```

# Solution 2: Prefix Sum (One Pass)

1. Initialize a dummy head node (result) to simplify edge cases, such as when the starting elements of the list sum to zero.
2. Traverse the linked list, calculating the cumulative or prefix sum of node values as you go.
3. Use a hash map to keep track of the first occurrence of each prefix sum and the corresponding node.
4. If a prefix sum is encountered that already exists in the map, it indicates the sublist from the next of the stored node up to the current node sums to zero. Remove this sublist by linking the stored node's next pointer to the current node's next.
5. After adjusting pointers, continue traversing the list from the next node.

## CODE

```cpp
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        unordered_map<int, ListNode*> prefixSumToNode;
        int prefixSum = 0;
        for (ListNode* current = dummy; current != nullptr; current = current->next) {
            prefixSum += current->val;
            if (prefixSumToNode.count(prefixSum)) {
                ListNode* prev = prefixSumToNode[prefixSum];
                ListNode* toRemove = prev->next;
                int p = prefixSum + (toRemove ? toRemove->val : 0);
                while (p != prefixSum) {
                    prefixSumToNode.erase(p);
                    toRemove = toRemove->next;
                    p += (toRemove ? toRemove->val : 0);
                }
                prev->next = current->next;
            } else {
                prefixSumToNode[prefixSum] = current;
            }
        }
        return dummy->next;
    }
};
```