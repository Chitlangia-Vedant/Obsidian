# [220. Contains Duplicate III](https://leetcode.com/problems/contains-duplicate-iii/)

You are given an integer array `nums` and two integers `indexDiff` and `valueDiff`.

Find a pair of indices `(i, j)` such that:

- `i != j`,
- `abs(i - j) <= indexDiff`.
- `abs(nums[i] - nums[j]) <= valueDiff`, and

Return `true` _if such pair exists or_ `false` _otherwise_.

**Example 1:**

```
**Input:** nums = [1,2,3,1], indexDiff = 3, valueDiff = 0
**Output:** true
**Explanation:** We can choose (i, j) = (0, 3).
We satisfy the three conditions:
i != j --> 0 != 3
abs(i - j) <= indexDiff --> abs(0 - 3) <= 3
abs(nums[i] - nums[j]) <= valueDiff --> abs(1 - 1) <= 0
```

**Example 2:**

```
		**Input:** nums = [1,5,9,1,5,9], indexDiff = 2, valueDiff = 3
**Output:** false
**Explanation:** After trying all the possible pairs (i, j), we cannot satisfy the three conditions, so we return false.
```

**Constraints:**

- `2 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`
- `1 <= indexDiff <= nums.length`
- `0 <= valueDiff <= 109`

# Solution 1

**Step-by-step implementation:**

1. **Initialize a SortedSet**: We create an empty `SortedSet` to maintain elements within our sliding window in sorted order.
2. **Iterate through the array**: For each element `nums[i]` at index `i`:
    a. **Search for a valid pair**:
    - Use `bisect_left(v - valueDiff)` to find the index of the smallest element in the sorted set that is `>= nums[i] - valueDiff`
    - If such an element exists (i.e., `j < len(s)`), check if it's also `<= nums[i] + valueDiff`
    - If both conditions are met, we've found a valid pair and return `True`
    b. **Add current element**: Insert `nums[i]` into the sorted set
    c. **Maintain window size**:
    - If we've processed more than `indexDiff` elements (i.e., `i >= indexDiff`), remove the element that's now outside the window
    - The element to remove is `nums[i - indexDiff]` - the one that's exactly `indexDiff + 1` positions behind
3. **Return False**: If we complete the iteration without finding a valid pair, return `False`

## CODE

```cpp
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int indexDiff, int valueDiff) {
        // Use a sorted set to maintain a sliding window of elements
        // The set stores elements within the index range constraint
        set<long> windowSet;
      
        for (int i = 0; i < nums.size(); ++i) {
            // Find the smallest element in the set that is >= (nums[i] - valueDiff)
            // This helps us check if there's any element in range [nums[i] - valueDiff, nums[i] + valueDiff]
            auto lowerBoundIter = windowSet.lower_bound(static_cast<long>(nums[i]) - valueDiff);
          
            // If such element exists and it's <= (nums[i] + valueDiff), 
            // we found a pair satisfying both constraints
            if (lowerBoundIter != windowSet.end() && 
                *lowerBoundIter <= static_cast<long>(nums[i]) + valueDiff) {
                return true;
            }
          
            // Add current element to the sliding window
            windowSet.insert(static_cast<long>(nums[i]));
          
            // Maintain sliding window size by removing element that's out of index range
            // When i >= indexDiff, remove the element at position (i - indexDiff)
            if (i >= indexDiff) {
                windowSet.erase(static_cast<long>(nums[i - indexDiff]));
            }
        }
      
        return false;
    }
};
```

## DRY RUN

**Input:** `nums = [2, 3, 5, 4, 9]`, `indexDiff = 2`, `valueDiff = 1`

We'll maintain a sliding window of size at most `indexDiff + 1 = 3` using a SortedSet.

**Step-by-step execution:**

### **i = 0, nums[0] = 2:**

- SortedSet is empty `[]`
- Search for elements in range `[2-1, 2+1] = [1, 3]`
- No elements in set, so no match found
- Add 2 to set: `[2]`
- Window size (1) ≤ indexDiff, no removal needed

### **i = 1, nums[1] = 3:**

- SortedSet: `[2]`
- Search for elements in range `[3-1, 3+1] = [2, 4]`
- Binary search finds 2 at index 0
- Check: Is 2 ≤ 4? Yes! **Valid pair found: (0, 1)**
- Return `True`

The algorithm found that indices 0 and 1 form a valid pair because:

- `abs(0 - 1) = 1 ≤ indexDiff = 2` ✓
- `abs(2 - 3) = 1 ≤ valueDiff = 1` ✓

### **Alternative scenario to show window maintenance:**

Let's say we had `nums = [1, 5, 9, 1, 5]`, `indexDiff = 2`, `valueDiff = 3`:

**i = 0:** SortedSet: `[]` → `[1]` **i = 1:** SortedSet: `[1]` → `[1, 5]` (no match in range [2, 8]) **i = 2:** SortedSet: `[1, 5]` → `[1, 5, 9]` (no match in range [6, 12]) **i = 3:** SortedSet: `[1, 5, 9]`

- Search range [1-3, 1+3] = [-2, 4]
- Binary search finds 1 at index 0
- Check: Is 1 ≤ 4? Yes! **Valid pair found: (0, 3)?**
- Wait! Index difference: `abs(0 - 3) = 3 > indexDiff = 2`
- But our window only contains indices 1, 2, 3 (we need to remove nums[0])
- Remove nums[0] = 1: SortedSet becomes `[5, 9]`
- Search again in `[5, 9]` for range [-2, 4]: no match
- Add nums[3] = 1: `[1, 5, 9]`

This demonstrates how the sliding window ensures we only compare elements within `indexDiff` distance.

# Solution 2

Sliding window ensures only those indices are considered whose the absolute difference is at most k. We only consider k indices at a time. This fulfills the second condition.

Buckets are used to ensure that the absolute difference between two numbers is at most t. Let's take a deeper look at them.  
We (floor) divide each number by t+1 and put it in a bucket with key as the quotient.  
For example,

```csharp
[1,5,2,4,3,9,1,5,9], k = 2, t = 3

1 // (3+1) = 0
5 // (3+1) = 1
2 // (3+1) = 0
4 // (3+1) = 1
3 // (3+1) = 0
9 // (3+1) = 2

Here, Bucket[0] will contain numbers 0,1,2,3.
Bucket[1] will contain numbers 4,5,6,7.
Bucket[2] will contain numbers 8,9,10,11.

On observing carefully, we can see that the absolute difference
between any two numbers in any bucket is at most t, which is what we want.

Also, there can be a case where the neighbouring bucket has some number
whose absolute difference with a number in the current bucket is at most t.
For instance, 2 lies in Bucket[0] and 4 lies in Bucket[1] and 4 - 2 = 2 < 3 (=t).
This can only happen in neighbouring buckets. Therefore, we need to check for this too.
```

## CODE

```cpp
class Solution {
public:
    bool containsNearbyAlmostDuplicate(vector<int>& nums, int k, int t) {
        int n = nums.size();
        
        if(n == 0 || k < 0  || t < 0) return false;
        
        unordered_map<int,int> buckets;
        
        for(int i=0; i<n; ++i) {
            int bucket = nums[i] / ((long)t + 1);
            
			// For negative numbers, we need to decrement bucket by 1
			// to ensure floor division.
			// For example, -1/2 = 0 but -1 should be put in Bucket[-1].
			// Therefore, decrement by 1.
            if(nums[i] < 0) --bucket;
            
            if(buckets.find(bucket) != buckets.end()) return true;
            else {
                buckets[bucket] = nums[i];
                if(buckets.find(bucket-1) != buckets.end() && (long) nums[i] - buckets[bucket-1] <= t) return true;
                if(buckets.find(bucket+1) != buckets.end() && (long) buckets[bucket+1] - nums[i] <= t) return true;
                
                if(buckets.size() > k) {
                    int key_to_remove = nums[i-k] / ((long)t + 1);
                    
                    if(nums[i-k] < 0) --key_to_remove;
                    
                    buckets.erase(key_to_remove);
                }
            }
        }
        
        return false;
    }
};
```

		