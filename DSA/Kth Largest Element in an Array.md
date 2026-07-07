# [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)

Given an integer array `nums` and an integer `k`, return _the_ `kth` _largest element in the array_.

Note that it is the `kth` largest element in the sorted order, not the `kth` distinct element.

Can you solve it without sorting?

**Example 1:**

**Input:** nums = [3,2,1,5,6,4], k = 2
**Output:** 5

**Example 2:**

**Input:** nums = [3,2,3,1,2,4,5,5,6], k = 4
**Output:** 4

**Constraints:**

- `1 <= k <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

```Java
class Solution {
    public int findKthLargest(int[] nums, int k) {
        
    }
}
```

# Understanding:

```
Input: nums = [3,2,1,5,6,4], k = 2
Sorted Order: [1 2 3 4 5 6]

1st largest element = 6
2nd largest element = 5 (K = 2) - Ans
```

```
Input: nums = [3,2,3,1,2,4,5,5,6], k = 4
Sorted Order: [1 2 2 3 3 4 5 5 6]

1st largest element = 6
2nd largest element = 5
3rd largest element = 5
(If 3rd largest unique element - Ans: 4)
4th largest element = 4 - Ans
```

# Solution:

## (1) Ascending Sorting:

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

After Sorting:
[1 2 3 4 5 6]

N = 6
K = 2

Index = N-K = 6-2 = 4
OP: 5

Ans: arr[n-k]
```

TC: O(NlogN)
SC: O(1)

## (2) Descending Sorting:

```
Input: nums = [3,2,1,5,6,4], k = 2
Output: 5

After Sorting:
[6 5 4 3 2 1]

Index = K-1

Ans = arr[k-1]
```

TC: O(NlogN)
SC: O(1)

```
C++: sort(arr.begin(), arr.end(), greater<int>) - Descending Sort

Java: Arrays.sort(arr).Reverse(s_idx, e_idx) - Descending Sort
```
## (3) Max Heap/Min Heap

```
Element = [1 5 2]

PQ (Min): [root: 1 2 5]
PQ (Max): [root: 5 2 1]

By Default:

C++: PQ -> Max Heap
Java: PQ -> Min Heap
Python: PQ -> Min Heap
```

```
Values = [1 5 2 3 4]

Max Heap = [5 4 3 2 1], Root/Top = 5
Min Heap = [1 2 3 4 5], Root/Top = 1
```

```
Heapify Function:

If there are K elements in the Heap, 
To Re-Adjust the Heap, Whenever new element is added/removed
Heapify Function is called

TC: O(log K) - 1 Heapify
SC: O(K)
```
## Max Heap
Heap: Largest Element at Root

Pop for K-1 Times
Root/Top: Kth Largest Element

Accessing Top/Head: O(1) TC
### CODE:

```cpp
class Solution 
{
public:
    int findKthLargest(vector<int>& nums, int k) 
    {
    // PriorityQueue<Integer> pq = new PriorityQueue<Integer>(); - JAVA
        // pq.add(), pq.peek(), pq.poll()
        
        priority_queue<int> pq (nums.begin(), nums.end()); // Max-Heap
        int i = 0;
        
        // Dont Pop for K Times, Just Pop for K-1 Times, So That your Answer -> Kth Largest Element is on Top
        for (i=0; i<k-1; i++)
            pq.pop(); // Deleted (K-1) Times
            
        return pq.top(); // Kth Largest Element   
    }
};
```

TC: O(NlogN) + O(K-1)
SC: O(N)
## Min Heap

Heap: Smallest Element at Root

Pop: N-K Times

### CODE:

```cpp
class Solution 
{
public:
    int findKthLargest(vector<int>& nums, int k) 
    {
        // Kth Smallest - Max Heap
       // priority_queue<int> pq (nums.begin(), nums.end()); // max- heap
         priority_queue<int, vector<int>, greater<int>> pq; // min-heap 

         for (int val: nums) // O(N)
         {
             pq.push(val);
             
             if (pq.size() > k) 
                 pq.pop(); // log K - Heapify
         }
        
        return pq.top();        
    }  
};
```

TC: O(NlogK)
SC: O(K)
# JAVA:

```Java
class Solution {
    public int findKthLargest(int[] nums, int k) 
    {
        PriorityQueue<Integer> pq = new PriorityQueue<>(); // Min-Heap

        for (int i=0; i<nums.length; i++) // O(N)
        {
            pq.add(nums[i]);

            if (pq.size() > k) // O(log K) - Heapify
                pq.poll();
        }

        return pq.peek();  // Kth Largest Element will be at Top   - O(1)
    }
}
```

TC: O(N * log K)
SC: O(K)
# Py:

```python
def findKthLargest(self, nums: List[int], k: int) -> int:
	return heapq.nlargest(k, nums)[-1]


def findKthLargest(self, nums: List[int], k: int) -> int:
	if not nums or not k or k<0:
		return None
	
	minheap = []
	for num in nums:
		if len(minheap)<k:
			heapq.heappush(minheap, num)
		else:
			if num > minheap[0]:
				heapq.heappop(minheap)
				heapq.heappush(minheap, num)
	
	return minheap[0] # top of minHeap
```

TC: O(N * log K)
SC: O(K)