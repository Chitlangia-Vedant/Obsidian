 # [136. Single Number](https://leetcode.com/problems/single-number/)

Given a **non-empty** array of integers `nums`, every element appears _twice_ except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**
	
	**Input:** nums = [2,2,1]
	
	**Output:** 1

**Example 2:**
	
	**Input:** nums = [4,1,2,1,2]
	
	**Output:** 4

**Example 3:**
	
	**Input:** nums = [1]
	
	**Output:** 1

**Constraints:**

- `1 <= nums.length <= 3 * 104`
- `-3 * 104 <= nums[i] <= 3 * 104`
- Each element in the array appears twice except for one element which appears only once.
  
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        
    }
};
```

# Solution:

## (1) Brute Force Solution:

Two Nested Loops,

if (arr[i] != arr[j]), return that number

TC: O(N^2)
SC: O(1)

## (2) Sorting:

[2 1 2] ----> [1 2 2]
All Duplicate Numbers will be together

Check whether adjacent values are same or not

TC: O(NlogN)
SC: O(1)

## (3) Using Map:

Element: Frequency

```cpp
if freq == 1

    return ans
```

TC: O(N)
SC: O(N)

## (4) Using XOR:

[Bits Manipulation](DSA/Theory/Bits Manipulation)

Duplicate Value (Even Frequency): XOR: 0
Single Value: XOR: Single Value

### CODE:
XOR All Values
------> Even (Repeated Values): 0
------> Single Value: ANS

DRY RUN:

IP: [2,1,2]
OP: 1

XOR of All Values:
```
2^1^2
= 2^2^1
= 0^1
= 1 - OP
```

CODE:

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) 
    {
        int ans = 0;
        for (auto val: nums)
            ans ^= val;
                
        return ans;
    }
};
```

TC: O(N)
SC: O(1)

## (5) Using Set:

Array: [4 1 2 1 2]
OP: 4

Set: [4 1 2]

Sum of Set * 2 = Array with All Values Twice = 14
Sum of Array = 10

Ans = Sum of set*2 - Sum of Array = 14-10 = 4

TC: O(N)
SC: O(N)