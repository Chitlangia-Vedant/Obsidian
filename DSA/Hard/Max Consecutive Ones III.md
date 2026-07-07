# [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)

Given a binary array `nums` and an integer `k`, return _the maximum number of consecutive_ `1`_'s in the array if you can flip at most_ `k` `0`'s.

**Example 1:**

**Input:** nums = \[1,1,1,0,0,0,1,1,1,1,0], k = 2
**Output:** 6
**Explanation:** \[1,1,1,0,0,**1**,1,1,1,1,**1**]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Example 2:**

**Input:** nums = \[0,0,1,1,0,0,1,1,1,0,1,1,0,0,0,1,1,1,1], k = 3
**Output:** 10
**Explanation:** \[0,0,1,1,**1**,**1**,1,1,1,**1**,1,1,0,0,0,1,1,1,1]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

**Constraints:**

- `1 <= nums.length <= 105`
- `nums[i]` is either `0` or `1`.
- `0 <= k <= nums.length`

```cpp
class Solution {
public:
    int longestOnes(vector<int>& nums, int k) {
        
    }
};
```

# TRICK:

Original Question:
return the maximum number of consecutive 1s in the array if you can flip at most k 0s.

Same Question:
Find the length of largest subarray of 1 in a binary array if you can flip at most K zero


number_of_flip <= K - This Ques
number_of_flip <= 1 - Prev Ques

# Solution:
## Approach:

Sliding Window: SUPERSET

- Two Variable Pointers which can move in either directions
- Reason: Sliding Window


....i......j......

arr[i....j]: Window

Any random indices i and j will constitute a subarray, arr[i....j]: Subarray
i and j BOTH Are variables and movable in either directions

- Increase the Window Size
- Decrease the Window Size

Input: nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
Output: 6
Explanation: \[1,1,1,0,0,**1**,1,1,1,1,**1**]
Bolded numbers were flipped from 0 to 1. The longest subarray is underlined.

```
[0,1,1,1,1,0]: Flipped K = 2 Zeroes
[1,1,1,1,1,1]: Total - OP: 6
```

Ques: Largest Subarray in the Array which has at most 2 zeroes?
 arr = [1,1,1,0,0,0,1,1,1,1,0]


```
Option-1: [1,1,1,0,0]: Size = 5
Option-2: [1,0,0]: Size = 3
Option-3: [0,0,1,1,1,1]: Size = 6
Option-4: [0,1,1,1,1,0]: Size = 6
Option-5: [1,1,0,0]: Size = 4
```


Same Question:
Find the length of largest subarray of 1 in a binary array if you can flip at most K zero


Final Question:
Find the longest subarray with AT MOST K Zeroes

Reasoning:

That Subarray with At MOST K Zeroes
Will eventually be flipped into 1 to give your answer

"Find the longest subarray with AT MOST K Zeroes"

At Most K = 0 to K

Irrespective of i and j position,
arr[i...j] ----> Always a Subarray


Sliding Window:
....i.......j......

j = 0 to N
i = 0 

For Each A[i],
Find the longest subarray with At MOST K Zeroes

i......j

increment j: Expanding the Window
increment i: Shrinking the Window

# Approach:

## (1) if A\[i....j] has numberOfZeroes < K
continue to increment j

Meaning:

My Count < K
EXPAND My Window Size: j++
## (2) If A[i......j] has numberOfZeroes > K
continue to increment i

Meaning:

My Count > K
SHRINK My Window Size: i++

# CODE:

```cpp
class Solution {
    
    // i, j: Two Ends of Window
    // j++: Window Increasing: EXPAND
    // i++: Window Decreasing: SHRINK
    

    // nums = [1,1,1,0,0,0,1,1,1,1,0], k = 2
    //.                  i.         j
    // OP: 6
    // len = 11
    public int longestOnes(int[] nums, int k) 
    {
        int i=0, j;
        
        // j = 0 to N: Always Increasing
        // Goal: "Find the largest Subarray with AT MOST K 0s"


        for (j=0; j<nums.length; j++)
        {
            // If Found 0 -> Flip, k -> K-1
            // j = 3, k = 2->1
            // j = 4, k = 1->0
            // j = 5, k = 0->-1
            if (nums[j] == 0)
                k--;

            // K < 0 -> Number of Zeroes > K
            // count > K: Decrease Window Size
            // i++: Shrinking the Window
            if (k < 0 && nums[i++] == 0)
                k++; // i = 1,2,3,4,5, j = 11
            
        /*
            if (k<0)
            {
                if (nums[i]== 0){
                    k++;
                }
				i++;

            }
            
          */  
            
        }
        
        //System.out.println("i: " + i + " j: " + j); // i =5, j = 11
       // Length of longest subarray with AT MOST K Zeroes 
        return j-i; // 11-5 = 6
        
    }
}
```

TC - O(N) 
SC - O(1)