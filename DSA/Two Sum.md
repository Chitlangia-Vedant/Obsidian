# [1. Two Sum](https://leetcode.com/problems/two-sum/)

Given an array of integers `nums` and an integer `target`, return _indices of the two numbers such that they add up to `target`_.

You may assume that each input would have **_exactly_ one solution**, and you may not use the _same_ element twice.

You can return the answer in any order.

**Example 1:**

**Input:** nums = [2,7,11,15], target = 9
**Output:** [0,1]
**Explanation:** Because nums[0] + nums[1] == 9, we return [0, 1].

**Example 2:**

**Input:** nums = [3,2,4], target = 6
**Output:** [1,2]

**Example 3:**

**Input:** nums = [3,3], target = 6
**Output:** [0,1]

**Constraints:**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **Only one valid answer exists.**

**Follow-up:** Can you come up with an algorithm that is less than `O(n2)` time complexity?

```cpp
class Solution {

public:

    vector<int> twoSum(vector<int>& nums, int target) {

    }

};
```

# Solution 
 ```Java
 class Solution {
    public int[] twoSum(int[] nums, int target) 
    {
        int[] result = new int[2];
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        // Key: Element, Value: Index
        
        int i = 0;
        for (i=0; i<nums.length; i++)  // O(N)
            // target=6, nums[i] = 1, target- nums[i] = 5
        {
            if (map.containsKey(target-nums[i])) // O(1)
            {
                result[1] = i;
                result[0] = map.get(target-nums[i]); // O(1)
                return result;
            }
            
            map.put(nums[i], i); 
            // {3: 0 ---> 3:1}, {2:0, 7:1, 11:2, 15:3}
        }
        
        return result;
    }
}
 ```