# [75. Sort Colors](https://leetcode.com/problems/sort-colors/)

Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example 1:**

**Input:** nums = [2,0,2,1,1,0]
**Output:** [0,0,1,1,2,2]

**Example 2:**

**Input:** nums = [2,0,1]
**Output:** [0,1,2]

**Constraints:**

- `n == nums.length`
- `1 <= n <= 300`
- `nums[i]` is either `0`, `1`, or `2`.

**Follow up:** Could you come up with a one-pass algorithm using only constant extra space?

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        
    }
};
```

# Solution:

Expected - Optimal Solution:

TC: O(N) - One Pass
SC: O(1)

## (1) Internal Sort/Library Sort: Not Allowed, But One of the Solutions

Arrays.sort(arr): Java
sort(arr, arr+n): C++
sorted(list): Py

TC: O(NlogN)
SC: O(1)

## (2) Merge Sort

TC: O(NlogN)
SC: O(N)

## (3) Two Pass (Travel Array Twice)

Approach:

- Count the frequency of 0,1,2
- Store in Variables: SC: O(1)
- Put 0,1,2 in that Order of Count

```
Input: nums = [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]

Value        Count

0             2
1             2
2             2

Put 0 Two Times, Put 1 Two Times, Put 2 Two Times
OP: [0,0,1,1,2,2]
```

TC: O(N) + O(N)
SC: O(1)

### CODE:

```Java
class Solution {
    public void sortColors(int[] nums) 
    {
        int countZero = 0, countOne = 0, countTwo = 0;
        int i =0;
        int n = nums.length;
        
        for (i=0; i<n; i++) // O(N)
        {
            if (nums[i]==0)
                ++countZero;
            
            else if (nums[i] == 1)
                ++countOne;
            
            else 
                ++countTwo;                
        }
        
        
        // countZero + countOne + countTwo = N
        
        // Putting 0 
        for (i=0; i<countZero; i++)
            nums[i] = 0;

        // Putting 1
        for (i=0; i<countOne; i++)
            nums[countZero + i] = 1;

        // Putting 2
        for (i=0; i<countTwo; i++)
            nums[countZero + countOne + i] = 2;        
    }
}
```

TC: O(N) + O(N) - Two Pass 
SC: O(1)

## (4) One Pass (Travel Array Once)

Approach:

0,1,2

If I find 0 ------> Move to Left

If I find 2 ------> Move to Right

....0          ...1...       ....2

Move All 0 to left
Move All 2 to right
All 1 By Itself will be in the Middle

### CODE:

```Java
class Solution {
    public void sortColors(int[] nums) 
    {
        int i = 0; 
        int n = nums.length;
        int start = 0, end = n-1;
        
        while (i<=end)
        {
            if (nums[i] == 0) // Move to left
            {
                swap(nums, i, start);
                start++;
                i++;
            }
            
            else if (nums[i] == 1) // Dont Move
                i++;
                        
            else if (nums[i] == 2) // Move to Right
            {
                swap(nums, i, end);
                end--;
            }
        }
    }  
    
    
public void swap(int[] nums, int a, int b)
{
        int temp = nums[a];
        nums[a]=nums[b];
        nums[b]=temp;
}
    
}
```

TC: O(N)
SC: O(1)

