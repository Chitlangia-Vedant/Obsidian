# [350. Intersection of Two Arrays II](https://leetcode.com/problems/intersection-of-two-arrays-ii/)

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must appear as many times as it shows in both arrays and you may return the result in **any order**.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]
**Output:** [2,2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]
**Output:** [4,9]
**Explanation:** [9,4] is also accepted.

**Constraints:**

- `1 <= nums1.length, nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 1000`

**Follow up:**

- What if the given array is already sorted? How would you optimize your algorithm?
- What if `nums1`'s size is small compared to `nums2`'s size? Which algorithm is better?
- What if elements of `nums2` are stored on disk, and the memory is limited such that you cannot load all elements into the memory at once?

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        
    }
};
```

# Solution 1: Using Map

- Using HashMap to store occurrences of elements in the `nums1` array.
- Iterate `num` in `nums2` array, check if `mp[num] > 0` then append `num` to our answer and decrease `mp[num]` by one.
## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> mp;

        for (int x : nums1) {
            mp[x]++;      // frequency map
        }
        vector<int> result;

        for (int num : nums2) {
            if (mp[num]>0){
                result.push_back(num);
                mp[num]--;
            }
        }
        return result;
    }
};
```

- To optimize the space, we ensure `len(nums1) <= len(nums2)` by swapping `nums1` with `nums2` if `len(nums1) > len(nums2)`.

# Solution 2

1. **Sort Both Arrays**: Sorting helps in comparing elements of both arrays efficiently. By sorting both arrays, we can then use a two-pointer approach to find common elements.
2. **Initialize Pointers**: Use two pointers `i` and `j` to traverse through `nums1` and `nums2` respectively. Also, use a pointer `k` to keep track of the position in `nums1` where we store the result.
3. **Traverse Both Arrays**:
    - Compare the elements at the current positions of both pointers.
    - If the element in `nums1` is less than the element in `nums2`, increment pointer `i`.
    - If the element in `nums1` is greater than the element in `nums2`, increment pointer `j`.
    - If the elements are equal, it means we have found a common element. Store this element in `nums1[k]`, increment `i`, `j`, and `k`.
4. **Return the Result**: The result is stored in the first `k` positions of `nums1`. Use `Arrays.copyOfRange` to return this part of the array.
## Complexity
- **Time Complexity**: Sorting both arrays takes (O(n log n + m log m)), where (n) is the length of `nums1` and (m) is the length of `nums2`. The two-pointer traversal takes (O(n + m)). Thus, the overall time complexity is (O(n log n + m log m + n + m) = `O(n log n + m log m))`.
- **Space Complexity**: The space complexity is (O(1)) if we ignore the space used for sorting, as we are not using any extra space apart from the input arrays.
## Step by Step Explanation

**Example Input:**

```text
nums1 = [4, 9, 5]
nums2 = [9, 4, 9, 8, 4]
```

**Sorted Arrays:**

```text
nums1: [4, 5, 9]
nums2: [4, 4, 8, 9, 9]
```

|Step|nums1[i]|nums2[j]|Action|Result|
|---|---|---|---|---|
|1|4|4|Append 4 to result|[4]|
|2|5|4|Increment j (nums2[j] < nums1[i])|[4]|
|3|5|8|Increment i (nums1[i] < nums2[j])|[4]|
|4|9|8|Increment j (nums2[j] < nums1[i])|[4]|
|5|9|9|Append 9 to result|[4, 9]|
|6|end|end|End of iteration|[4, 9]|

**Final Result:**

```text
[4, 9]
```
## CODE

```cpp
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(), nums1.end());
        sort(nums2.begin(), nums2.end());
        
        int i = 0, j = 0;
        vector<int> result;
        
        while (i < nums1.size() && j < nums2.size()) {
            if (nums1[i] < nums2[j]) {
                i++;
            } else if (nums1[i] > nums2[j]) {
                j++;
            } else {
                result.push_back(nums1[i]);
                i++;
                j++;
            }
        }
        
        return result;
    }
};
```