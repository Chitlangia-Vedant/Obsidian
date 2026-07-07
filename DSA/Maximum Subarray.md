# [53. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)

Given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example 1:**

**Input:** nums = [-2,1,-3,4,-1,2,1,-5,4]
**Output:** 6
**Explanation:** The subarray [4,-1,2,1] has the largest sum 6.

**Example 2:**

**Input:** nums = [1]
**Output:** 1
**Explanation:** The subarray [1] has the largest sum 1.

**Example 3:**

**Input:** nums = [5,4,-1,7,8]
**Output:** 23
**Explanation:** The subarray [5,4,-1,7,8] has the largest sum 23.

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`

**Follow up:** If you have figured out the `O(n)` solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

```
class Solution {

public:

    int maxSubArray(vector<int>& nums) {

    }

};
```
# Solution:
  
Input:
[-2, -1, -3]
  
OP:

**Containing at least 1 element:**
Subarrays:
```
[-2]: -2

[-2 -1]: -3

[-2 -1 -3]: -6

[-1]: -1

[-1 -3]: -4

[-3]: -3
```

Max Sum: -1

---
**If Empty Subarray is allowed:**
Subarrays:
```
[]: 0

[-2]: -2

[-2 -1]: -3

[-2 -1 -3]: -6

[-1]: -1

[-1 -3]: -4

[-3]: -3
```

Max Sum: 0
# Approach:

If Empty Subarray allowed:
```
[]: Sum = 0
```

Solutions:
1. Brute Force
2. Kadane (Sliding Window)
## (1) Brute Force:

Find Sum of All subarrays, then find Max

TC: O(N^2)
SC: O(1)
## (2) Optimised Solution

------> Kadane Algo:
- Sliding Window:

(1) Start traversing the array

(2) For each element ----->

Keep: `currSum `and `overallSum`

(3) Find Max of All Sum: `overallSum`

Edge Case:

```
if (currSum < 0)
   currSum = 0;
```

TC: O(N)
SC: O(1)
### CODE:

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        int maxSum = INT_MIN; // -2^31
        // Empty Subarray is Allowed,[-2 -3 -1]: OP: 0
        // int maxSum = 0; // -2^31
        int currSum = 0;

        // [-2, -3, -1]
        int i = 0;
        for (i=0; i<n; i++)
        {
            currSum += nums[i]; 
            // currSum = -2, currSum = -3, currSum = -1
            maxSum = std::max(currSum, maxSum);
            // maxSum = -2, maxSum = -2, maxSum = -1

            if (currSum < 0)
                currSum = 0; // currSum = 0
        }

        return maxSum; // -1
    }
};
```