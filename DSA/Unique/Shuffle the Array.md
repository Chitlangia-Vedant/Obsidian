# [1470. Shuffle the Array](https://leetcode.com/problems/shuffle-the-array/)

Given the array `nums` consisting of `2n` elements in the form `[x1,x2,...,xn,y1,y2,...,yn]`.

_Return the array in the form_ `[x1,y1,x2,y2,...,xn,yn]`.

**Example 1:**

**Input:** nums = [2,5,1,3,4,7], n = 3
**Output:** [2,3,5,4,1,7] 
**Explanation:** Since x1=2, x2=5, x3=1, y1=3, y2=4, y3=7 then the answer is [2,3,5,4,1,7].

**Example 2:**

**Input:** nums = [1,2,3,4,4,3,2,1], n = 4
**Output:** [1,4,2,3,3,2,4,1]

**Example 3:**

**Input:** nums = [1,1,2,2], n = 2
**Output:** [1,2,1,2]

**Constraints:**

- `1 <= n <= 500`
- `nums.length == 2n`
- `1 <= nums[i] <= 10^3`

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        
    }
};
```

# Solution 1: Iteration (Extra Space)

1. Create an empty result array.
2. Loop `i` from `0` to `n - 1`:
    - Append `nums[i]` (the x value) to the result.
    - Append `nums[i + n]` (the corresponding y value) to the result.
3. Return the result array.

## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        
        vector<int> res(2*n);
        for(int i=0;i<n;i++){
            res[2*i]=nums[i];
            res[2*i+1]=nums[n+i];
                
        }
        return res;
    }
};
```

## Time & Space Complexity

- Time complexity: O(n)
- Space complexity: O(n) extra space.

# Solution 2: Multiplication And Modulo

## Intuition

- To shuffle in-place without extra space, we can encode two values in a single array element. 
- Since values are at most 1000, we pick a base `M > 1000`. 
- For each position `i` in the result, we determine which original element belongs there and encode it by adding `original_value * M` to `nums[i]`. 
- The original value is retrieved using `nums[index] % M` (since we have not yet divided), and the new value is stored in the upper part. 
- After encoding all positions, we divide each element by `M` to extract the shuffled values.
## Algorithm

1. Choose `M = max(nums) + 1` or a constant like `1001`.
2. For each index `i` from `0` to `2n - 1`:
    - If `i` is even, the value should come from position `i / 2` (an `x` value).
    - If `i` is odd, the value should come from position `n + i / 2` (a `y` value).
    - Add `(nums[source] % M) * M` to `nums[i]`.
3. Divide each element by `M` to get the final shuffled array.
4. Return the modified array.

## CODE

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        int M = *max_element(nums.begin(), nums.end()) + 1;
        for (int i = 0; i < 2 * n; i++) {
            if (i % 2 == 0) {
                nums[i] += (nums[i / 2] % M) * M;
            } else {
                nums[i] += (nums[n + i / 2] % M) * M;
            }
        }
        for (int i = 0; i < 2 * n; i++) {
            nums[i] /= M;
        }
        return nums;
    }
};
```

## Time & Space Complexity

- Time complexity: O(n)
- Space complexity: O(1) extra space.

# Solution 3: Bit Manipulation

## Intuition

- Similar to the multiplication approach, we can use bit manipulation to pack two values into one integer. 
- Since values fit in 10 bits (max 1000 < 1024 = 2^10), we can store one value in the lower 10 bits and another in the upper bits. 
- First, we pack each `x` and its corresponding `y` into the first half of the array. 
- Then we unpack them from back to front into their final interleaved positions, ensuring we do not overwrite values we still need.

## Algorithm

1. For each `i` from `0` to `n - 1`:
    - Combine `nums[i]` (`x`) and `nums[i + n]` (`y`) into `nums[i]` using: `nums[i] = (nums[i] << 10) | nums[i + n]`.
2. Starting from `i = n - 1` down to `0`, and using a pointer `j` starting at `2n - 1`:
    - Extract `y = nums[i] & ((1 << 10) - 1)`.
    - Extract `x = nums[i] >> 10`.
    - Place `nums[j] = y` and `nums[j - 1] = x`.
    - Decrement `j` by 2.
3. Return the modified array.

## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        for (int i = 0; i < n; i++) {
            nums[i] = (nums[i] << 10) | nums[i + n]; // Store x, y in nums[i]
        }

        int j = 2 * n - 1;
        for (int i = n - 1; i >= 0; i--) {
            int y = nums[i] & ((1 << 10) - 1);
            int x = nums[i] >> 10;
            nums[j] = y;
            nums[j - 1] = x;
            j -= 2;
        }

        return nums;
    }
};
```

## Time & Space Complexity

- Time complexity: O(n)
- Space complexity: O(1) extra space.