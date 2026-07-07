# [813. Largest Sum of Averages](https://leetcode.com/problems/largest-sum-of-averages/)

You are given an integer array `nums` and an integer `k`. You can partition the array into **at most** `k` non-empty adjacent subarrays. The **score** of a partition is the sum of the averages of each subarray.

Note that the partition must use every integer in `nums`, and that the score is not necessarily an integer.

Return _the maximum **score** you can achieve of all the possible partitions_. Answers within `10-6` of the actual answer will be accepted.

**Example 1:**

**Input:** nums = [9,1,2,3,9], k = 3
**Output:** 20.00000
**Explanation:** 
The best choice is to partition nums into [9], [1, 2, 3], [9]. The answer is 9 + (1 + 2 + 3) / 3 + 9 = 20.
We could have also partitioned nums into [9, 1], [2], [3, 9], for example.
That partition would lead to a score of 5 + 2 + 6 = 13, which is worse.

**Example 2:**

**Input:** nums = [1,2,3,4,5,6,7], k = 4
**Output:** 20.50000

**Constraints:**

- `1 <= nums.length <= 100`
- `1 <= nums[i] <= 104`
- `1 <= k <= nums.length`

```cpp
class Solution {
public:
    double largestSumOfAverages(vector<int>& nums, int k) {
        
    }
};
```

## Intuition

To maximize the sum of averages, we need to understand a key mathematical property: dividing an array into more groups generally increases the total score. This is because the average of averages tends to be higher than the average of the whole. For instance, `[9]/1 + [1]/1 = 10` is greater than `[9,1]/2 = 5`.

This suggests we should use as many partitions as possible (up to `k`) to maximize our score. However, we can't just split arbitrarily - we need to be strategic about where we make the cuts.

The problem has an optimal substructure property: if we decide to make the first subarray from index `0` to some index `j-1`, then the remaining problem becomes finding the best way to partition `nums[j:]` into at most `k-1` groups. This naturally leads to a [dynamic programming](https://algo.monster/problems/dynamic_programming_intro) approach.

We can think of this recursively: at each position `i`, we try all possible endpoints for the current subarray (from `i+1` to `n`), calculate the average of that subarray, and add it to the best possible score for partitioning the remaining array with one fewer group allowed.

To efficiently calculate subarray sums, we can use a [prefix sum](https://algo.monster/problems/subarray_sum) array `s` where `s[i]` represents the sum of elements from index `0` to `i-1`. This allows us to compute the sum of any subarray from index `i` to `j-1` as `s[j] - s[i]` in constant time.

The base cases are straightforward:

- When we've processed all elements (`i = n`), the score is `0`
- When we have only one group left (`k = 1`), we must take all remaining elements as a single subarray

By using memoization with `@cache`, we avoid recalculating the same subproblems multiple times, making the solution efficient.
### Solution Approach

The solution uses **[prefix sum](https://algo.monster/problems/subarray_sum)** and **memoized search ([dynamic programming](https://algo.monster/problems/dynamic_programming_intro))** to efficiently find the maximum score.

**Step 1: Build [Prefix Sum](https://algo.monster/problems/subarray_sum) Array**

First, we create a [prefix sum](https://algo.monster/problems/subarray_sum) array `s` using `accumulate(nums, initial=0)`. This gives us an array where `s[i]` represents the sum of all elements from index `0` to `i-1`. With this preprocessing, we can calculate the sum of any subarray from index `i` to `j-1` as `s[j] - s[i]` in O(1) time.

**Step 2: Define the Recursive Function**

We define `dfs(i, k)` which returns the maximum sum of averages when partitioning the array starting from index `i` into at most `k` groups.

The function works as follows:

1. **Base Case 1**: When `i == n`, we've reached the end of the array, so we return `0` (no more elements to partition).
    
2. **Base Case 2**: When `k == 1`, we have only one group left, so we must take all remaining elements as a single subarray. The average is `(s[n] - s[i]) / (n - i)`.
    
3. **Recursive Case**: For each possible endpoint `j` of the current subarray (from `i+1` to `n`):
    
    - Calculate the average of the current subarray: `(s[j] - s[i]) / (j - i)`
    - Add the maximum score for partitioning the remaining array: `dfs(j, k - 1)`
    - Track the maximum among all possible choices

**Step 3: Memoization**

The `@cache` decorator automatically memoizes the results of `dfs(i, k)`, preventing redundant calculations. This is crucial because the same subproblem `(i, k)` may be encountered multiple times through different recursion paths.

**Step 4: Return the Result**

We call `dfs(0, k)` to start the partitioning from index `0` with at most `k` groups allowed.

**Time Complexity**: O(n² × k) where n is the length of the array, as we have n possible values for `i`, k possible values for `k`, and each state requires O(n) time to compute.

**Space Complexity**: O(n × k) for the memoization cache plus O(n) for the recursion stack.

## Example Walkthrough

Let's walk through the solution with `nums = [4, 1, 7]` and `k = 2`.

**Step 1: Build Prefix Sum Array**

- `s = [0, 4, 5, 12]` (cumulative sums: 0, 0+4, 0+4+1, 0+4+1+7)

**Step 2: Calculate `dfs(0, 2)`** (partition entire array with at most 2 groups)

We need to decide where to end the first subarray. We have three options:

1. First subarray = `[4]`, remaining = `[1, 7]`
2. First subarray = `[4, 1]`, remaining = `[7]`
3. First subarray = `[4, 1, 7]`, remaining = `[]`

**Option 1**: First subarray ends at index 1

- Average of `[4]` = `(s[1] - s[0]) / (1 - 0)` = `4/1 = 4`
- Recursively solve `dfs(1, 1)` for `[1, 7]` with 1 group left
    - With k=1, must take all remaining: average = `(s[3] - s[1]) / (3 - 1)` = `8/2 = 4`
- Total score: `4 + 4 = 8`

**Option 2**: First subarray ends at index 2

- Average of `[4, 1]` = `(s[2] - s[0]) / (2 - 0)` = `5/2 = 2.5`
- Recursively solve `dfs(2, 1)` for `[7]` with 1 group left
    - With k=1, must take all remaining: average = `(s[3] - s[2]) / (3 - 2)` = `7/1 = 7`
- Total score: `2.5 + 7 = 9.5`

**Option 3**: First subarray ends at index 3

- Average of `[4, 1, 7]` = `(s[3] - s[0]) / (3 - 0)` = `12/3 = 4`
- Recursively solve `dfs(3, 1)` for `[]` (empty array)
    - Base case: i=n, return 0
- Total score: `4 + 0 = 4`

**Step 3: Choose Maximum**

- Maximum of (8, 9.5, 4) = **9.5**

Therefore, the optimal partition is `[4, 1], [7]` with a maximum score of 9.5.

The memoization ensures that if we encounter the same `(i, k)` pair again (which doesn't happen in this small example), we reuse the computed result instead of recalculating.

## Solution Implementation

```cpp
class Solution {
public:
    double largestSumOfAverages(vector<int>& nums, int k) {
        int n = nums.size();
      
        // Prefix sum array to calculate range sums efficiently
        // prefixSum[i] = sum of nums[0] to nums[i-1]
        vector<int> prefixSum(n + 1, 0);
        for (int i = 0; i < n; ++i) {
            prefixSum[i + 1] = prefixSum[i] + nums[i];
        }
      
        // Memoization table for dynamic programming
        // memo[i][j] = maximum sum of averages starting from index i with j groups remaining
        vector<vector<double>> memo(n, vector<double>(k + 1, -1.0));
      
        // Recursive function with memoization to find maximum sum of averages
        // Parameters:
        //   startIdx: starting index of current subarray
        //   groupsLeft: number of groups we can still create
        // Returns: maximum sum of averages from startIdx to end with groupsLeft groups
        function<double(int, int)> dfs = [&](int startIdx, int groupsLeft) -> double {
            // Base case: reached end of array
            if (startIdx == n) {
                return 0.0;
            }
          
            // Base case: only one group left, must take all remaining elements
            if (groupsLeft == 1) {
                int remainingSum = prefixSum[n] - prefixSum[startIdx];
                int remainingCount = n - startIdx;
                return static_cast<double>(remainingSum) / remainingCount;
            }
          
            // Check if already computed
            if (memo[startIdx][groupsLeft] >= 0) {
                return memo[startIdx][groupsLeft];
            }
          
            double maxSum = 0.0;
          
            // Try all possible end positions for current group
            // We need at least groupsLeft-1 elements after endIdx for remaining groups
            for (int endIdx = startIdx + 1; endIdx <= n - groupsLeft + 1; ++endIdx) {
                // Calculate average of current group [startIdx, endIdx)
                int currentSum = prefixSum[endIdx] - prefixSum[startIdx];
                int currentCount = endIdx - startIdx;
                double currentAverage = static_cast<double>(currentSum) / currentCount;
              
                // Recursively calculate maximum for remaining elements
                double remainingMax = dfs(endIdx, groupsLeft - 1);
              
                // Update maximum sum
                maxSum = max(maxSum, currentAverage + remainingMax);
            }
          
            // Store result in memoization table and return
            return memo[startIdx][groupsLeft] = maxSum;
        };
      
        // Start recursion from index 0 with k groups
        return dfs(0, k);
    }
};
```

## Time and Space Complexity

The time complexity is `O(n² × k)` where `n` is the length of the array `nums`.

This complexity arises from the memoized recursive function `dfs(i, k)`:

- There are `n × k` possible states (combinations of starting index `i` from 0 to n-1, and partition count `k` from 1 to k)
- For each state `dfs(i, k)`, we iterate through positions `j` from `i+1` to `n-1`, which takes `O(n)` time in the worst case
- Therefore, the total time complexity is `O(n × k) × O(n) = O(n² × k)`

The space complexity is `O(n × k)`.

This is due to:

- The memoization cache stores up to `n × k` states from the recursive calls
- The recursion stack depth is at most `O(k)` (we make at most k partitions)
- The prefix sum array `s` takes `O(n)` space
- Since `k ≤ n`, the dominant factor is the memoization cache, giving us `O(n × k)` space complexity