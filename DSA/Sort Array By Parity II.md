# [922. Sort Array By Parity II](https://leetcode.com/problems/sort-array-by-parity-ii/)

Given an array of integers `nums`, half of the integers in `nums` are **odd**, and the other half are **even**.

Sort the array so that whenever `nums[i]` is odd, `i` is **odd**, and whenever `nums[i]` is even, `i` is **even**.

Return _any answer array that satisfies this condition_.

**Example 1:**

**Input:** nums = [4,2,5,7]
**Output:** [4,5,2,7]
**Explanation:** [4,7,2,5], [2,5,4,7], [2,7,4,5] would also have been accepted.

**Example 2:**

**Input:** nums = [2,3]
**Output:** [2,3]

**Constraints:**

- `2 <= nums.length <= 2 * 104`
- `nums.length` is even.
- Half of the integers in `nums` are even.
- `0 <= nums[i] <= 1000`

**Follow Up:** Could you solve it in-place?

```cpp
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& nums) {
        
    }
};
```

## Approach 1: Separate even and odd numbers

The next approach that might come to mind is to just iterate over the array and separate the numbers into two groups, even and odd.  
Then we build the array again from the start by choosing an even number for each even index and an odd number for each odd index.  
This is a good solution, and it does fit in with the least runtime possible for such an algorithm, which is **O(n)** since we have to at least check every element, so that they are all at their place, but it also requires extra space of O(n) for storing the even and odd numbers.

The code for the approach is as follows:

## CODE

```cpp
vector<int> sortByParityII(vector<int>& nums) {
	int n = nums.size();
	vector<int> evens, odds;
	evens.reserve(n/2);
	odds.reserve(n/2);
	for(int i = 0; i<n; i++) {
		if(nums[i] % 2 == 0) {
			evens.push_back(nums[i]);
		else {
			odds.push_back(nums[i]);
	}
	//filling even spaces
	for(int i = 0; i<n; i+=2) {
		nums[i] = evens[i/2];
	}
	//filling odd spaces
	for(int i = 1; i<n; i+=2) {
		nums[i] = odds[i/2];
	}
	return nums;
}
```

If you are confused by the lines `evens.reserve(n/2)` and `odds.reserve(n/2)`, they are just ensuring beforehand that our vectors have adequate capacities, so that all push back operations are completed in O(1).

**Time: O(n)** as we just iterate the array two times.  
**Space: O(n)**, the extra space required for the two arrays.  
  

## Approach 2: Swap outliers

We can think of this question in another way as well. Since the number of even and odd numbers in the array is equal, if there is one outlier (even number at odd index or vice versa), there must be another outlier somewhere, since if there isn't, then this place has no rightful element which can fill it.  
If we find and swap such pairs, we wouldn't have to care about finding the odd or even numbers and separating them. In fact, we can even do it in one iteration only, using two pointers.

Coming back to the question statement, as n is even,, so index 0 is even and the index (n-1) is odd. So starting at index 0 for the even pointer, and 1 for the odd pointer would be just fine.  
We follow the below algorithm until one of the pointers reaches the other end of the array (here i represents the even pointer, and j represents the odd pointer):

1. Keep incrementing i by 2 until either it reaches the other end, or an outlier (odd number at even index).
2. Keep incrementing j by 2 until either it reaches the other end, or an outlier (even number at odd index).
3. Swap nums[i] and nums[j]

The code for this approach is as follows:

```cpp
class Solution {
public:
    vector<int> sortArrayByParityII(vector<int>& A) {
    for (int i = 0, j = 1; i < A.size(); i += 2, j += 2) {
        while (i < A.size() && A[i] % 2 == 0) i += 2;
        while (j < A.size() && A[j] % 2 == 1) j += 2;
        if (i < A.size()) swap(A[i], A[j]);
    }
    return A;
}
};
```

**Time: O(n)** as we traverse the array once.  
**Space: O(1)** as we only use two pointers, and no other space.