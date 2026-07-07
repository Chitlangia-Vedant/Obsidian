# [Next Greater Element](https://www.geeksforgeeks.org/problems/next-larger-element-1587115620/1)

You are given an array **arr[]** of integers, the task is to find the **next greater element** for each element of the array in order of their appearance in the array. Next greater element of an element in the array is the nearest element on the **right** which is **greater** than the current element.  
If there does not exist next greater of current element, then next greater element for current element is **-1**.

**Examples**

**Input:** arr[] = [1, 3, 2, 4]
**Output:** [3, 4, 4, -1]
**Explanation:** The next larger element to 1 is 3, 3 is 4, 2 is 4 and for 4, since it doesn't exist, it is -1.

**Input**: arr[] = [6, 8, 0, 1, 3]
**Output**: [8, -1, 1, 3, -1]
**Explanation**: The next larger element to 6 is 8, for 8 there is no larger elements hence it is -1, for 0 it is 1, for 1 it is 3 and then for 3 there is no larger element on right and hence -1.

**Input**: arr[] = [1, 2, 3, 5]
**Output**: [2, 3, 5, -1]
**Explanation**: For a sorted array, the next element is next greater element also except for the last element.

**Input**: arr[] = [5, 4, 3, 1]
**Output**: [-1, -1, -1, -1]
**Explanation**: There is no next greater element for any of the elements in the array, so all are -1.

**Constraints:**  
1 ≤ arr.size() ≤ 106  
0 ≤ arr[i] ≤ 109

**Expected Complexities**

Time Complexity: O(n)
Auxiliary Space: O(n)

```cpp
class Solution {
  public:
    vector<int> nextLargerElement(vector<int>& arr) {
        // code here
        
    }
};
```

# Understanding
Given an Array, 
Print the "Immediately" Next Greater Element on the right side for each element

If No NGE, print -1

```
arr = [4 5 2 25]
OP: [5, 25, 25, -1]

4 -> [5 2 25]: 5
5 -> [2 25]: 25
2 -> [25]: 25
25 -> []: -1


Hint: Last Value ------> Always -1
```

# Solution:

Leaders in an Array:

Maximum Value on Right
(Travel from R to L, Keep Max and compare and update)

Immediate Greater Value on Right:
- That Approach wont work
## (1) Brute Force: Two Nested Loops

```
arr = [4 5 2 25]

4 -> [5 2 25]
Immediate NGE for 4: 5

OP: [5 25 25 -1]
```

```cpp
int[] nge(int[] arr , int n)
{
	int[] res = new int[n];
	for (i=0; i<n; i++)
	{
		for (j=i+1; j<n; j++)
		{
			if (arr[j] > arr[i])
			{
				res[i] = arr[j];
				break;
			}
		}
		res[i] = -1;
	}
	return res;
}
```

TC: O(N^2)
SC: O(1)

## (2) Optimised Solution: Using Stack

Why Stack:
Ordering Required for "IMMEDIATE" Next Greater Element -----> Stack Required

Approach:

(1) Go from R to L
(2) Pop from Stack till get Next Greater Element on Top
	OR
	Stack becomes Empty
		Intuition: Dont need smaller or Equal Values
(3) If Stack Empty: ans: -1
(4) Else: Ans: stack.top()

### CODE:

```cpp
arr = [4 5 2 25]
OP: [5 25 25 -1]


int[] nge(int[] arr , int n)
{
	stack<int> s;
	int res[n];

	// R to L
	for (i=n-1; i>=0; i--)
	// arr[i] = 25, 2, 5, 4
	{
		// 25 <= 2 : FALSE
		// 2 <= 5: TRUE
		// 25 <= 5: FALSE
		// 5 <= 4: FALSE
		while (!s.empty() && s.top()<=arr[i])
			s.pop(); // s = [TOP: 25] 

		if (s.empty())
			res[i] = -1; // res = [_, _, _, -1]

		else
			res[i] = s.top(); // res = [5, 25, 25, -1]

		s.push(arr[i]); // s = [TOP: 4, 5, 25]
	}

	return res; // res = [5, 25, 25, -1]
}
```

TC: O(N)
SC: O(N)