# [485. Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/)

Given a binary array `nums`, return _the maximum number of consecutive_ `1`_'s in the array_.

**Example 1:**

**Input:** nums = [1,1,0,1,1,1]
**Output:** 3
**Explanation:** The first two digits or the last three digits are consecutive 1s. The maximum number of consecutive 1s is 3.

**Example 2:**

**Input:** nums = [1,0,1,1,0,1]
**Output:** 2

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.

```cpp
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        
    }
};
```

# Solution:

## Trick:
"return the maximum number of consecutive 1s in the array" ->"Find the length of largest subarray of 1 in a binary array"

## Approach:

A Variable ans to find largest subarray of 1

```
if (arr[i]==0)
	reset ans = 0
else
	ans++;
```

Final Ans:
Max of All Ans

Because there can be multiple subarrays of 1, we need to find Max

## DRY RUN:

```
Input: nums = [1,1,0,1,1,1]
Output: 3

ans = 0

[1,1,0,1,1,1]
 ^

 ans: 0 -> 1

[1,1,0,1,1,1]
   ^

 ans: 1 -> 2

[1,1,0,1,1,1]
     ^

 ans: 0 : RESET

[1,1,0,1,1,1]
       ^

 ans: 0 -> 1

[1,1,0,1,1,1]
         ^

 ans: 1 -> 2

[1,1,0,1,1,1]
           ^

 ans: 2 -> 3


Final Ans = max(2,3) = 3
```

# CODE:

```Java
class Solution 
{
    public int findMaxConsecutiveOnes(int[] nums) 
    {
        int ans = 0;
        int res = Integer.MIN_VALUE;

    // ans = 2, ans = 3
    // res = max(2,3) = 3
        for (int val: nums)
            res = Math.max(res, ans = val == 0 ? 0 : ans+1);

        return res;
    }
}
```

TC: O(N)
SC: O(1)
