# [303. Range Sum Query - Immutable](https://leetcode.com/problems/range-sum-query-immutable/)

Given an integer array `nums`, handle multiple queries of the following type:

1. Calculate the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** where `left <= right`.

Implement the `NumArray` class:

- `NumArray(int[] nums)` Initializes the object with the integer array `nums`.
- `int sumRange(int left, int right)` Returns the **sum** of the elements of `nums` between indices `left` and `right` **inclusive** (i.e. `nums[left] + nums[left + 1] + ... + nums[right]`).

**Example 1:**

**Input**
["NumArray", "sumRange", "sumRange", "sumRange"]
[[[-2, 0, 3, -5, 2, -1]], [0, 2], [2, 5], [0, 5]]
**Output**
[null, 1, -1, -3]

**Explanation**
NumArray numArray = new NumArray([-2, 0, 3, -5, 2, -1]);
numArray.sumRange(0, 2); // return (-2) + 0 + 3 = 1
numArray.sumRange(2, 5); // return 3 + (-5) + 2 + (-1) = -1
numArray.sumRange(0, 5); // return (-2) + 0 + 3 + (-5) + 2 + (-1) = -3

**Constraints:**

- `1 <= nums.length <= 104`
- `-105 <= nums[i] <= 105`
- `0 <= left <= right < nums.length`
- At most `104` calls will be made to `sumRange`.

```cpp
class NumArray {
public:
    NumArray(vector<int>& nums) {
        
    }
    
    int sumRange(int left, int right) {
        
    }
};
```

# Solution 1

## CODE (Mine)

```cpp
class NumArray {
private:
    vector<int> nums;
public:
    NumArray(vector<int>& nums): nums{nums}{
    }
    
    int sumRange(int left, int right) {
        return accumulate(nums.begin()+left,nums.begin()+right+1,0);
    }
};
```

# Solution 2

- Pre-calculated the prefix sum of `nums` array, where `preSum[i]` is sum of `nums[0..i]`, so the result of `sumRange(left, right)` are:
    - If `left = 0` then return `preSum[right]`
    - Else return `preSum[right] - preSum[left-1]`.

## CODE

```cpp
class NumArray { // 12 ms, faster than 99.87%
public:
    vector<int>& preSum; // `preSum` will reference to `nums` array, no copy at all!
    
    NumArray(vector<int>& nums) : preSum(nums) {
        for (int i = 1; i < preSum.size(); ++i)
            preSum[i] += preSum[i-1]; 
    }
    
    int sumRange(int left, int right) {
        if (left == 0) return preSum[right];
        return preSum[right] - preSum[left-1];
    }
};
```

## **Complexity**

- Time:
    - Constructor: O(N)
    - sumRange(left, right): `O(1)`
- Space: `O(1)`

If you think this **post is useful**, I'm happy if you **give a vote**. Any **questions or discussions are welcome!** Thank a lot.
