# [128. Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/)

Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example 1:**

**Input:** nums = [100,4,200,1,3,2]
**Output:** 4
**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

**Example 2:**

**Input:** nums = [0,3,7,2,5,8,4,6,0,1]
**Output:** 9

**Example 3:**

**Input:** nums = [1,0,1,2]
**Output:** 3

**Constraints:**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

```Java
class Solution {
    public int longestConsecutive(int[] nums) {
        
    }
}
```
# Understanding:

Input: nums = [100,4,200,1,3,2]
Output: 4

[1 2 3 4]

- Not a Subarray
- Not a Subsequence
- Subset

(1) Ordering is NOT Important
Need Subset: Not Subsequence ot Subarray

(2) Duplicates

```
[1 2 2 3 4]
OP: 4

[1 2 3 4]
OP: 4
```
Duplicates will not change answer

# Solution:

## (1) Brute Force: Sorting

Sorting Advantage:
- Increasing Order, Easy to check for longest consecutive sequence
- Same Values will be together, Easy to check for same adjacent elements


```
int count = 0;

if (nums[i] == nums[i+1] + 1)
	++count;

if (nums[i] == nums[i+1])
	continue; // Skip, Duplicate Elements

if (nums[i]!= nums[i-1] + 1)
	Reset count = 0
	Keep track of Maximum
	More than 1 consecutive Sequence can exist
```

### CODE:

```Java
class Solution 
{
    public int longestConsecutive(int[] nums) 
    {
        int len = nums.length;
        if (len == 0)
            return 0;

        Arrays.sort(nums); // O(NlogN)
        int count = 0, curr_len = 1;
        int i = 0;

        for (i=1; i<len; i++) // O(N)
        {
            // Duplicates -> skip, no change in count
            if (nums[i] == nums[i-1])
                continue;

            // Consecutive -> Increase by 1
            else if (nums[i] == nums[i-1] + 1)
                curr_len += 1;

            // Reset count = 0
            // Keep Track of Maximum
            // More than 1 Consecutive Subsequences Exist
            else
            {
                count = Math.max(count, curr_len);
                curr_len = 1;
            }     
        }

        return Math.max(count, curr_len);
    }
}
```

TC: O(NlogN)
SC: O(1)

## (2) Ordered Set

In C++
	Default set : Ordered set

```
Example:

Input:nums = [0,3,7,2,5,8,4,6,0,1]
Ordered Set: s = [0,1,2,3,4,5,6,7,8] // Removed duplicate and order
Check the longest consecutive elements sequence
Output: 9
```
### CODE

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        if (nums.empty())
            return 0;

        set<int> s(begin(nums), end(nums)); // Ordered Set

        int count = 1;
        int maxc = 1;
        int temp = *s.begin();

        for (auto val : s) {
            if (val == temp + 1) {
                count++;
            } else if (val != temp) {
                count = 1;
            }

            temp = val;
            maxc = max(maxc, count);
        }

        return maxc;
    }
};
```

TC: O(NlogN)
SC: O(N)
## (3) Optimised Solution: Unordered Set

O(N): Iterate Over Array

Traverse the Array and Check in Set if ele-1 exists or not.
If it exists, ++cnt;

Keep Maximum of that length of consecutive sequence

Set Lookup: O(1)

```
arr = [100,4,200,1,3,2]
set = (100,4,200,1,3,2)

4: curr_len = 1
Check if 4-1=3 exists in set- YES

3: curr_len = 2
Check if 3-1=2 exists in set- YES

2: curr_len = 3
Check if 2-1=1 exists in set- YES

1: curr_len = 4
Check if 1-1=0 exists in set - NO
```

TC: O(N)
SC: O(N)
### CODE:

#### C++:

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) 
    {
        unordered_set<int> s(begin(nums), end(nums));
        int count = 0;
        
        // s.count() ---> 0(Not Exist) or 1(Exist) in Set
        
        for (auto &val: s)
        {
            // Skip values till you reach val-1
            if (s.count(val-1))
                continue;
            
            // After val-1, you are at val, start count from 1
                int curr_len = 1;
            
            // val +1, val + 2, val + 3 --> Sequence Starts
                while (s.count(val + curr_len))
                    curr_len++;
                
                count = max(count, curr_len);
        }   
        return count;
    }
};
```

TC: O(N)
SC: O(N) - Set