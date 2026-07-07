https://leetcode.com/problems/wiggle-sort/description/ (LC Premium)

https://www.lintcode.com/problem/508/description
# 508 · Wiggle Sort

## Description

Given an unsorted array `nums`, reorder it **in-place** such that

```
nums[0] <= nums[1] >= nums[2] <= nums[3]....
```

There may have multiple answers for a question, **you only need to consider one of the possibilities**.

Please sort the array in place and do not define additional arrays.

## Example

**Example 1:**

```
Input: [3, 5, 2, 1, 6, 4]
Output: [1, 6, 2, 5, 3, 4]
Explanation: This question may have multiple answers, and [2, 6, 1, 5, 3, 4] is also ok.
```

**Example 2:**

```
Input: [1, 2, 3, 4]
Output: [1, 4, 2, 3]
```

```Java
public class Solution {
    /**
     * @param nums: A list of integers
     * @return: nothing
     */
    public void wiggleSort(int[] nums) {
        // write your code here
    }
}
```

# Solutions:

## (1) Sorting:

Intuition behind Sorting:

```
Pattern After Sorting: <= <= <= <=
Need Pattern: <= >= <= >= 
```

Odd Index: Higher ------> Swapping

- Sort the Array
- Increase Index by 2
- Swap the values

Edge Cases:

(1) arr.length = 1
No Wiggle

[1], [2]

(2) arr.length = 2

[2 1] ------> [1 2]

if (arr[0] > arr[1]), swap
else -> No change

### CODE:

 ```Java
    public void wiggleSort(int[] nums) 
    {
        int n = nums.length;

        if (n < 2)
            return;

        Arrays.sort(nums); // O(NlogN)
        int i = 1; // Odd Index Swapping

        for (i=1; i<n-1; i+=2) // O(N/2)
        {
            // Swap the values: Odd Index should be higher: Swapping
            int temp = nums[i];
            nums[i] = nums[i+1];
            nums[i+1] = temp;
        }
    }
 ```


TC: O(NlogN) + O(N/2) = O(NlogN)
SC: O(1)

## (2) Optimised Solution: Without Sorting

Pattern: Number in Odd Index: PEAK

```
If Index is Odd: arr[i] >= arr[i-1], else swap
				  ODD   >= EVEN	


If Index is Even: arr[i] <= arr[i-1], else swap
				  EVEN   <= ODD	
```

### CODE:

```Java
public void wiggleSort(int[] nums) 
    {
        if (nums.length < 2)
            return;

        for (int i=0; i<nums.length-1; i++)
        {
            // If Index is Even
            if (i %2 == 0)
            {
                if (nums[i] >= nums[i+1]) // Even Index Val > Odd Index Value
                {
                    swap(nums, i, i+1);
                }
            }

            else //  // If Index is Odd
            {
                if (nums[i] <= nums[i+1]) // Odd Index Val < Even Index Val
                {
                    swap(nums, i, i+1);
                }
            }

        }

        return;
    }


    public void swap(int[] nums, int i, int j)
    {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
```

TC: O(N)
SC: O(1)