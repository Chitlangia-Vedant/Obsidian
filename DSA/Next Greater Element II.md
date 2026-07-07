# [503. Next Greater Element II](https://leetcode.com/problems/next-greater-element-ii/)

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return _the **next greater number** for every element in_ `nums`.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

**Example 1:**

**Input:** nums = [1,2,1]
**Output:** [2,-1,2]
Explanation: The first 1's next greater number is 2; 
The number 2 can't find next greater number. 
The second 1's next greater number needs to search circularly, which is also 2.

**Example 2:**

**Input:** nums = [1,2,3,4,3]
**Output:** [2,3,4,-1,4]

**Constraints:**

- `1 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`

```Java
class Solution {
    public int[] nextGreaterElements(int[] nums) {
        
    }
}
```

# Understanding:

Input: nums = [1,2,1]
Output: [2,-1,2]

Circular Array = [1,2,1,1,2,1,1,2,1,1,2,1,.......]

NGE for 1: Iterate Once: Ans: 2

NGE for 2: Iterate Twice: Ans: -1

NGE for 1: Iterate Twice: Ans: 2

# Solution:

NGE in Circular Array
= NGE in Normal Array: Min Iterating Twice

Iterate twice in Array: 2 Ways:

## (1) Create Extra Array:

int[] b = new int[2*n];

SC: O(2*N)

## (2) Use Modulo Operator:

```
for (i=0; i<2*n; i++)
	arr[i%n]
```

SC: O(1)

```
nums = [1,2,1]
n = 3

for (i=0; i<6; i++)
	nums[i % 3]


i = 0, nums[0%3] = nums[0] = 1
i = 1, nums[1%3] = nums[1] = 2
i = 2, nums[2%3] = nums[2] = 1
i = 3, nums[3%3] = nums[0] = 1
i = 4, nums[4%3] = nums[1] = 2
i = 5, nums[5%3] = nums[2] = 1
```

# CODE:

```java
class Solution 
{
    public int[] nextGreaterElements(int[] nums) 
    {
        int n = nums.length;
        int[] res = new int[n];
        int i = 0;

        for (i=0; i<n; i++)
            res[i] = -1;

        Stack<Integer> stack = new Stack<Integer>();

    // NGE in Circular Array = NGE in Normal Array: Twice Iteration
        for (i=0; i<2*n; i++)
        {
            // Avoid Index Out of Bound
            int val = nums[i%n];

            while(!stack.isEmpty() && nums[stack.peek()] < val)
            {
                res[stack.peek()] = val;
                stack.pop();
            }

            if (i<n)
                stack.push(i);
        }
        
        return res;
    }
}
```


TC: O(2*N)
SC: O(N) - Stack