# [23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

You are given an array of `k` linked-lists `lists`, each linked-list is sorted in ascending order.

_Merge all the linked-lists into one sorted linked-list and return it._

**Example 1:**

```
**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted linked list:
1->1->2->3->4->4->5->6
```
**Example 2:**

```
**Input:** lists = []
**Output:** []
```
**Example 3:**

```
**Input:** lists = [[]]
**Output:** []
```

**Constraints:**

- `k == lists.length`
- `0 <= k <= 104`
- `0 <= lists[i].length <= 500`
- `-104 <= lists[i][j] <= 104`
- `lists[i]` is sorted in **ascending order**.
- The sum of `lists[i].length` will not exceed `104`.

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
    public ListNode mergeKLists(ListNode[] lists) {
        
    }
}
```

# Solution:

Hints:
1 - Extend Merge 2 List Approach to merge K List Approach
2 - Try to think in terms of Arrays

Problem Statement:
Given K Sorted Arrays/LL, Sort them together

N: Total Number of Elements in all LL (Cumulative)
K: Number of Arrays/LL

```
Input: [[1,2], [2,3],[3,4]]

N = 6, K = 3
```

Edge Cases:
- any of the ll/arrays can be empty? yes
- they are all sorted
- duplicates exist? yes
- all arrays/ll have different sizes? yes

# Approaches:

## (1) Extend Approach for merging 2 sorted arrays/list

For 2 Arrays/lists:
```
TC: O(list1 + list2) = O(2*list1) [Same Size]
```

Same Approach:
K Lists:

```
TC: O(list1 + list2 + ......... + listk)
TC: O(K*N) [N: Size of 1 List Element]
```

## (2) Use PQ (Min Heap)

Intuition/Context:
Find Kth Smallest/Largest Values in an Array

Solution: 
Priority Queue (Min Heap)

TC: O(NlogK)
SC: O(K)

Approach:
1 - Create a Min Heap 
2 - Create a final result ll/array to have final sorted ll/array
3 - Insert first node/value of all ll/arrays
4 - Heap Size: K
5 - Pop from heap, insert into final answer
6 - Keep adding to heap, HEAP WILL TAKE CARE OF FINDING THE SMALLEST ELEMENT, 
Hence we do not need to follow the process in approach 1 - Optimisation in TC
7 - At any given instance, my heap would contain only K Elements
8 - Repeat step 5-7 till heap is empty
9 - Return your final ans

### CODE:

```Java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        // Example 2 and Example 3
        if (lists==null || lists.length==0) 
            return null;

        // Min-Heap
        PriorityQueue<ListNode> queue= new PriorityQueue<ListNode>(lists.length, (a,b)-> a.val-b.val);
        
        ListNode result = new ListNode(0); // final ans
        ListNode tail=result;
        
        // Insert 1st element/head of All the K lists in Min Heap
        for (ListNode node:lists)
            if (node!=null)
                queue.add(node);
            
        while (!queue.isEmpty()){
            tail.next=queue.poll();
            tail=tail.next;
            
            if (tail.next!=null)
                queue.add(tail.next);
        }

        return result.next; // 0 was a delimiter, hence returned dummy.next
    }
}
```


TC: O(NlogK) - N: Total Number of Nodes
SC: O(K) : K: Number of Lists

N * K vs N * log K

N = 50
K = 10

N*k = 500
N*logk = 50*0.3010 (base 10) =~ 15

## Intuition

The goal is to merge sorted linked lists into one single sorted linked list. Since each individual list is already sorted, the smallest overall element must be at the head of one of these lists.

Using a **Priority Queue (Min-Heap)** allows us to efficiently find and remove the smallest element among all current list heads. Once we pick the smallest node and add it to our result, we simply replace it in the queue with the next node from that same list.

---

## Approach

1. **Initialize Min-Heap:** Create a `PriorityQueue` that stores `ListNode` objects, ordered by their `val`.
2. **Initial Population:** Iterate through the `lists` array and add the head of every non-null list into the queue.
3. **Dummy Node:** Use a dummy node (`head`) to simplify the construction of the result list. Use a `res` pointer to track the tail of this new list.
4. **Extract and Refill:**

- While the queue is not empty:
- `poll()` the smallest node (`curr`) from the queue.
- Attach `curr` to `res.next`.
- Move the `res` pointer forward.
- **Refill:** If `curr` has a `next` node, `offer()` that next node into the queue.

5. **Return:** The merged list starts at `head.next`.

### Dry Run Analogy

Imagine lines of people, each sorted by height. To form one single line sorted by height, you look at the person at the front of every line. You pick the shortest one and put them in the new line. Then, the person who was standing behind them moves to the front of their respective line, and you repeat the process.

---

## Complexity

- **Time complexity:** — Where is the total number of nodes and is the number of linked lists. Each of the nodes is added to and removed from the heap exactly once, and heap operations take .
- **Space complexity:** — The priority queue stores at most one node from each of the lists at any given time.

```cpp
class Solution {
public:
    struct compare {
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val;
        }
    };
    
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<ListNode*, vector<ListNode*>, compare> pq;
        for (auto l : lists) {
            if (l) pq.push(l);
        }
        
        ListNode* dummy = new ListNode(0);
        ListNode* tail = dummy;
        
        while (!pq.empty()) {
            ListNode* curr = pq.top();
            pq.pop();
            tail->next = curr;
            tail = tail->next;
            if (curr->next) pq.push(curr->next);
        }
        return dummy->next;
    }
};
```
## (3) Quick Select:

Pre-Req: Hoares Quick Select Algo

Optimal Solution to Find
- Partition for Pivot Element

TC: O(N) - Best Case
SC: O(1)

Tricky Part:
Worst Case:
TC: O(N^2) - Worst Case

Workarounds:
Use Median of Median Approach - Works with Best Case and Avg Case