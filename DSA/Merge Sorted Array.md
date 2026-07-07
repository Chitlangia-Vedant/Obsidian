# [88. Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/)

You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example 1:**

**Input:** nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
**Output:** [1,2,2,3,5,6]
**Explanation:** The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.

**Example 2:**

**Input:** nums1 = [1], m = 1, nums2 = [], n = 0
**Output:** [1]
**Explanation:** The arrays we are merging are [1] and [].
The result of the merge is [1].

**Example 3:**

**Input:** nums1 = [0], m = 0, nums2 = [1], n = 1
**Output:** [1]
**Explanation:** The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.

**Constraints:**

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-109 <= nums1[i], nums2[j] <= 109`

**Follow up:** Can you come up with an algorithm that runs in `O(m + n)` time?

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        
    }
};
```

# Solution 1: Two Pointers

Steps
1. Two pointer `i` and `j` which initially point to the first element of `nums1` and `nums2`
2. Compare `nums1[i]` and `nums2[j]`
	1. If `nums1[i]<nums2[j]`, then push `nums[i]` in `res` and `i++`
	2. If `nums1[i]>nums2[j]`, then push `nums[j]` in `res` and `j++`
	3. Loop until either `nums1` or `nums2` is empty
3. Push the remaining elements of non empty array in `res`
4. `nums1=res`
## CODE (Mine)

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        vector<int> res;
        int i=0,j=0;
        while(i<m&&j<n){
            if(nums1[i]<nums2[j]){
                res.push_back(nums1[i++]);
            }else{
                res.push_back(nums2[j++]);
            }
        }
        while(i<m){
            res.push_back(nums1[i++]);
        }
        while(j<n){
            res.push_back(nums2[j++]);
        }
        nums1=res;
        return;
    }
};
```

# Solution 2: _**Using STL**_

1. Traverse through nums2 and append its elements to the end of nums1 starting from index m.
2. Sort the entire nums1 array using sort() function.
## Complexity
- Time complexity: `O((m+n)log(m+n))`
> _due to the sort() function_

- Space complexity: O(1)
> _We are not using any extra space, so the space complexity is O(1)._

## CODE

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        for (int j = 0, i = m; j<n; j++){
            nums1[i] = nums2[j];
            i++;
        }
        sort(nums1.begin(),nums1.end());
    }
};
```

# Solution 3: Two Pointer Reverse

1. Start with two pointers i and j, initialized to m-1 and n-1, respectively. 
2. Another pointer k initialized to m+n-1, which will be used to keep track of the position in nums1 where we will be placing the larger element. 
3. Then we can start iterating from the end of the arrays, and compare the elements at these positions. 
4. We will place the larger element in nums1 at position k, and decrement the corresponding pointer i or j accordingly. 
5. We will continue doing this until we have iterated through all the elements in nums2. If there are still elements left in nums1, we don't need to do anything because they are already in their correct place.

## Complexity

- Time complexity: O(m+n)
> _We are iterating through both arrays once, so the time complexity is O(m+n)._

- Space complexity: O(1)
> _We are not using any extra space, so the space complexity is O(1)._

## CODE

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        
        while (j >= 0) {
            if (i >= 0 && nums1[i] > nums2[j]) {
                nums1[k--] = nums1[i--];
            } else {
                nums1[k--] = nums2[j--];
            }
        }
    }
};
```