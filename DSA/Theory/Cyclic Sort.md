- Cyclic sort is a technique used to efficiently sort a collection of numbers when the elements are within a specific range. 
- It works by placing each number at its correct index, cycling through the array until all elements are in their right positions. 
- This approach is especially useful for problems where the array contains elements in the range from 1 to n, where n is the length of the array.
- It works in O(n) time complexity and uses constant space.

## How it works

The key to cyclic sort is ensuring that each number is placed at the index corresponding to its value. The algorithm continuously swaps elements until all numbers are in their correct places. The core steps are:

1. Iterate through the array.
2. For each number, if it is not at its correct index (i.e., the index does not match its value), swap it with the number at its target index.
3. Repeat the process until all numbers are in their correct positions.

# Example 1: Sorting an array of consecutive numbers

Given an array of numbers where each number is in the range from 1 to n, sort the array in-place (without using extra space) in O(n)O(n) time. Below is an implementation of cyclic sort to do this.

```Python
def cyclic_sort(nums):
    i = 0
    while i < len(nums): 
        # Since numbers are from 1 to n, the correct index for num is num-1
        correct_idx = nums[i] - 1 

        if nums[i] != nums[correct_idx]: 
            # Swap to place the number at the correct index
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
        else:
            i += 1

    return nums
```

![[Pasted image 20260702190353.png]]

# Example 2: Find the smallest missing positive number

Given an unsorted array of integers, find the smallest missing positive number. To solve this, we cyclic sort the array, then find the smallest index where the number is not correct.

```Python
def find_smallest_missing_positive(nums):
    n = len(nums)

    # Cyclic sort to place numbers in their correct index
    i = 0
    while i < n:
        correct_idx = nums[i] - 1
        # Only swap if nums[i] is in the range [1, n] and not already in the correct position
        if 1 <= nums[i] <= n and nums[i] != nums[correct_idx]:
            nums[i], nums[correct_idx] = nums[correct_idx], nums[i]
        else:
            i += 1

    # Now find the first index where the number is not correct
    for i in range(n):
        if nums[i] != i + 1:
            return i + 1  # The smallest missing positive number

    # If all positions are correct, return n + 1
    return n + 1
```

![[Pasted image 20260702190457.png]]