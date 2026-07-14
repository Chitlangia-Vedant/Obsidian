# [2462. Total Cost to Hire K Workers](https://leetcode.com/problems/total-cost-to-hire-k-workers/)

You are given a **0-indexed** integer array `costs` where `costs[i]` is the cost of hiring the `ith` worker.

You are also given two integers `k` and `candidates`. We want to hire exactly `k` workers according to the following rules:

- You will run `k` sessions and hire exactly one worker in each session.
- In each hiring session, choose the worker with the lowest cost from either the first `candidates` workers or the last `candidates` workers. Break the tie by the smallest index.
    - For example, if `costs = [3,2,7,7,1,2]` and `candidates = 2`, then in the first hiring session, we will choose the `4th` worker because they have the lowest cost `[3,2,7,7,**1**,2]`.
    - In the second hiring session, we will choose `1st` worker because they have the same lowest cost as `4th` worker but they have the smallest index `[3,**2**,7,7,2]`. Please note that the indexing may be changed in the process.
- If there are fewer than candidates workers remaining, choose the worker with the lowest cost among them. Break the tie by the smallest index.
- A worker can only be chosen once.

Return _the total cost to hire exactly_ `k` _workers._

**Example 1:**

**Input:** costs = [17,12,10,2,7,2,11,20,8], k = 3, candidates = 4
**Output:** 11
**Explanation:** We hire 3 workers in total. The total cost is initially 0.
- In the first hiring round we choose the worker from [17,12,10,2,7,2,11,20,8]. The lowest cost is 2, and we break the tie by the smallest index, which is 3. The total cost = 0 + 2 = 2.
- In the second hiring round we choose the worker from [17,12,10,7,2,11,20,8]. The lowest cost is 2 (index 4). The total cost = 2 + 2 = 4.
- In the third hiring round we choose the worker from [17,12,10,7,11,20,8]. The lowest cost is 7 (index 3). The total cost = 4 + 7 = 11. Notice that the worker with index 3 was common in the first and last four workers.
The total hiring cost is 11.

**Example 2:**

**Input:** costs = [1,2,4,1], k = 3, candidates = 3
**Output:** 4
**Explanation:** We hire 3 workers in total. The total cost is initially 0.
- In the first hiring round we choose the worker from [1,2,4,1]. The lowest cost is 1, and we break the tie by the smallest index, which is 0. The total cost = 0 + 1 = 1. Notice that workers with index 1 and 2 are common in the first and last 3 workers.
- In the second hiring round we choose the worker from [2,4,1]. The lowest cost is 1 (index 2). The total cost = 1 + 1 = 2.
- In the third hiring round there are less than three candidates. We choose the worker from the remaining workers [2,4]. The lowest cost is 2 (index 0). The total cost = 2 + 2 = 4.
The total hiring cost is 4.

**Constraints:**

- `1 <= costs.length <= 105`
- `1 <= costs[i] <= 105`
- `1 <= k, candidates <= costs.length`
# Solution : Greedy + Two Min Heaps
### Intuition

In each hiring session, we can only choose a worker from one of two groups:

- the first `candidates` remaining workers
- the last `candidates` remaining workers

To efficiently find the cheapest available worker from each side, we maintain **two min-heaps**:

- `pq1` stores candidates from the left side.
- `pq2` stores candidates from the right side.

The smallest element of each heap is always the cheapest worker available from that side.

Initially, we fill both heaps with up to `candidates` workers while ensuring that the same worker is never inserted twice (when the two ranges overlap).

After hiring a worker:

- If the worker came from the left heap, we insert the next unseen worker from the left.
- If the worker came from the right heap, we insert the next unseen worker from the right.

This keeps both heaps representing the current set of eligible candidates throughout all hiring sessions.

When both heaps contain workers with the same minimum cost, we always choose the worker from the **left heap**. Since workers are inserted in increasing index order from the left and decreasing index order from the right, this correctly satisfies the tie-breaking rule of choosing the smaller index.

---

## Algorithm

1. Let `n` be the number of workers.
2. Create two min-heaps:
    - `pq1` for the left candidates.
    - `pq2` for the right candidates.
3. Initialize two pointers:
    - `i = 0` (leftmost unseen worker)
    - `j = n - 1` (rightmost unseen worker)
4. Fill both heaps with up to `candidates` workers:
    - Insert workers from the left into `pq1`.
    - Insert workers from the right into `pq2`.
    - If both pointers meet, insert the worker only once to avoid duplicates.
5. Repeat `k` hiring sessions:
    - If one heap is empty, hire from the other heap.
    - Otherwise:
        - Compare the minimum costs from both heaps.
        - Hire the worker with the smaller cost.
        - If the costs are equal, hire from the left heap to satisfy the tie-breaking rule.
    - Add the hired worker's cost to the answer.
6. After removing a worker:
    - If the worker came from the left heap and unseen workers remain, insert the next worker from the left.
    - If the worker came from the right heap and unseen workers remain, insert the next worker from the right.
7. After completing all `k` sessions, return the accumulated cost.

---

### Why Two Pointers?

The pointers `i` and `j` keep track of workers that have **not yet been added** to either heap.

- Every time we hire from the left, we advance `i` and add the next left worker.
- Every time we hire from the right, we decrement `j` and add the next right worker.

This ensures:

- Every worker is inserted at most once.
- The heaps always contain exactly the currently eligible candidates.

---

### Why Does Tie-Breaking Work?

The problem states that if two workers have the same hiring cost, we must choose the one with the **smaller index**.

When the minimum costs of both heaps are equal:

- Every worker in `pq1` comes from the left side.
- Every worker in `pq2` comes from the right side.

Since the left pointer always processes smaller indices before the right pointer, every worker in `pq1` has a smaller index than every worker currently in `pq2`.

Therefore, choosing from `pq1` whenever the costs are equal correctly implements the required tie-breaking rule.
## CODE (Mine)

```cpp
class Solution {
public:
    long long totalCost(vector<int>& costs, int k, int candidates) {
        int n=costs.size();
        priority_queue<int,vector<int>,greater<int>> pq1,pq2;
        int i=0,j=n-1;
        while(i<candidates&&i<=j){
            if(i==j){
                pq1.push(costs[i]);
                i++;
                j--;
                continue;
            }
            pq1.push(costs[i]);
            i++;
            pq2.push(costs[j]);
            j--;
        }
        long long sum=0;
        for(;k>0;k--){
            if (pq1.empty()) {
                sum+=pq2.top();
                pq2.pop();
                if(i<=j){
                    pq2.push(costs[j]);
                    j--;                      
                }
            }
            else if (pq2.empty()) {
                sum+=pq1.top();
                pq1.pop();
                if(i<=j){
                    pq1.push(costs[i]);
                    i++;   
                }
            }
            else if(pq1.top()<=pq2.top()){
                sum+=pq1.top();
                pq1.pop();
                if(i<=j){
                    pq1.push(costs[i]);
                    i++;   
                }
            }else{
                sum+=pq2.top();
                pq2.pop();
                if(i<=j){
                    pq2.push(costs[j]);
                    j--;                      
                }
            }
        }
        return sum;
    }
};
```

### Time & Space Complexity

- **Time complexity:** `O((candidates + k) log(candidates))`
    - Initial heap construction inserts at most `2 × candidates` workers.
    - Each of the `k` hiring sessions performs at most one removal and one insertion into a heap, each taking `O(log(candidates))`.
- **Space complexity:** `O(candidates)`

> Where `n` is the length of `costs`. Each heap stores at most `candidates` workers, so the total extra space is proportional to `candidates`.