# [189. Rotate Array](https://leetcode.com/problems/rotate-array/)

Given an integer array `nums`, rotate the array to the right by `k` steps, where `k` is non-negative.

**Example 1:**

**Input:** nums = [1,2,3,4,5,6,7], k = 3
**Output:** [5,6,7,1,2,3,4]
**Explanation:**
rotate 1 steps to the right: [7,1,2,3,4,5,6]
rotate 2 steps to the right: [6,7,1,2,3,4,5]
rotate 3 steps to the right: [5,6,7,1,2,3,4]

**Example 2:**

**Input:** nums = [-1,-100,3,99], k = 2
**Output:** [3,99,-1,-100]
**Explanation:** 
rotate 1 steps to the right: [99,-1,-100,3]
rotate 2 steps to the right: [3,99,-1,-100]

**Constraints:**

- `1 <= nums.length <= 105`
- `-231 <= nums[i] <= 231 - 1`
- `0 <= k <= 105`

**Follow up:**

- Try to come up with as many solutions as you can. There are at least **three** different ways to solve this problem.
- Could you do it in-place with `O(1)` extra space?
  
```
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        
    }
};
```

# Solution:
## Pattern/Template: 4 Variations

(1) Rotate to Right, K > 0
(2) Rotate to Left, K > 0
(3) Rotate to Right, K < 0
(4) Rotate to Left, K < 0

1,4 and 2,3 - Same

nums = [1,2,3,4,5,6,7]

K = 0
OP: [1,2,3,4,5,6,7]

K = N or Multiple of N -----> K % N == 0
OP: [1,2,3,4,5,6,7] - Same

K > N
OP: K % N Times Rotation

K < N
OP: K % N Times Rotation

N = 3
K = 10
OP: 10%3 = 1 Rotation

[1 2 3]
N = 10

OP: [3 1 2]

Effective Rotations = 10%3 = K % N

# Solution:

## (1) With Extra Space

```
res[n]

arr[(i+k)%n] = res[i] // RIGHT SHIFT

arr[(i-k)%n] = res[i] // LEFT SHIFT
```

TC: O(N)
SC: O(N)
### CODE:

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) 
    {
        int n = nums.size();
        vector<int> res(n); // Extra Array/Vector
        
        if (n==0 || (k<=0) || (k%n==0))
            return;
        
        int i = 0;
        for (i=0; i<n; i++)
        {
            res[i] = nums[i];
        }
        
        
        for (i=0; i<n; i++)
        {
            nums[(i+k)%n] = res[i]; 
            // i ----> i + k: RIGHT SHIFT
            // i ----> i - k: LEFT SHIFT
        }
    }
};
```


```Output
nums = [1 2 3]
K = 2
OP: [2 3 1]
res = [1 2 3]
Inside Loop:

i = 0
nums[(0+2)%3] = res[0]
nums[2] = res[0]
nums[2] = 1

i = 1
nums[(1+2)%3] = res[1]
nums[0] = res[1]
nums[0] = 2

i = 2
nums[(2+2)%3] = res[2]
nums[1] = res[2]
nums[1] = 3

nums = [2 3 1] - OP
```
## (2) Without Extra Space

k %= nums.size() - Effective Number of Rotations

### Approach:
- Reverse Complete Array
- Reverse Array from 0 to K
- Reverse Array from K to N

```
Input: nums = [1,2,3,4,5,6,7], k = 3
Output: [5,6,7,1,2,3,4]

nums.size() = 7
K%N = 3%7 = 3

(1) reverse(nums.begin(), nums.end()) // O(N)
reverse = [7 6 5 4 3 2 1]
(2) reverse(nums.begin(). nums.begin() + K) // O(K)
reverse = [5 6 7 4 3 2 1]
(3) reverse(nums.begin() + K, nums.end()) // O(N-K)
reverse = [5 6 7 1 2 3 4] - OP
```

In Place Solution: SC: O(1)
Modify Original Array, No Extra Array Created

TC: O(N) + O(N)
SC: O(1)

### CODE:

```cpp
class Solution {
  public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
  }
  
  public void reverse(int[] nums, int start, int end) {
    while (start < end) {
      int temp = nums[start];
      nums[start] = nums[end];
      nums[end] = temp;
      start++;
      end--;
    }
  }
}
```

TC: O(N) + O(N)
SC: O(1)