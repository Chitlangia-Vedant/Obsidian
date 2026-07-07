https://leetcode.com/problems/max-consecutive-ones-ii/ (LC Premium)
https://www.lintcode.com/problem/883/description
# Description

Given a binary array, find the maximum number of consecutive 1s in this array if you can flip at most one 0.

Candidate Written Test Screening, Team Competency Assessment, Programming Teaching Exercises, Online Exam Grading

WeChat for information

1. The input array will only contain `0` and `1`.
2. The length of input array is a positive integer and will not exceed `10,000`.

Example

```
Example 1:
	Input:  nums = [1,0,1,1,0]
	Output:  4
	
	Explanation:
	Flip the first zero will get the the maximum number of consecutive 1s.
	After flipping, the maximum number of consecutive 1s is 4.

Example 2:
	Input: nums = [1,0,1,0,1]
	Output:  3
	
	Explanation:
	Flip each zero will get the the maximum number of consecutive 1s.
	After flipping, the maximum number of consecutive 1s is 3.
	
```

# Understanding:

Condition: 
If you can flip at most one 0
[1,0,1,1,0]

Curr ans, without flipping: 2

There are 2 zeroes available to flip, Index: 1 and 4

If I flip at index 4:
[1,0,1,1,1], Ans: 3

If I flip at Index 1:
[1,1,1,1,0], Ans: 4

Final Ans = max(3,4) = 4

# Solution:

## TRICK:

Original Question:
return the maximum number of consecutive 1s in the array if you can flip at most one 0.

Same Question:
Find the length of largest subarray of 1 in a binary array if you can flip at most one zero


# CODE:


```cpp
public int longestOnes(int[] nums) 
{
	int i=0, j;
	int k = 1;
	
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
```