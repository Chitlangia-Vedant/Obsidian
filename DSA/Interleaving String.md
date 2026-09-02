[97. Interleaving String](https://leetcode.com/problems/interleaving-string/)

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where `s` and `t` are divided into `n` and `m` substrings respectively, such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

**Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
**Output:** true
**Explanation:** One way to obtain s3 is:
Split s1 into s1 = "aa" + "bc" + "c", and s2 into s2 = "dbbc" + "a".
Interleaving the two splits, we get "aa" + "dbbc" + "bc" + "a" + "c" = "aadbbcbcac".
Since s3 can be obtained by interleaving s1 and s2, we return true.

**Example 2:**

**Input:** s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
**Output:** false
**Explanation:** Notice how it is impossible to interleave s2 with any other string to obtain s3.

**Example 3:**

**Input:** s1 = "", s2 = "", s3 = ""
**Output:** true

**Constraints:**

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`, `s2`, and `s3` consist of lowercase English letters.

**Follow up:** Could you solve it using only `O(s2.length)` additional memory space?

## Key Points to Consider

### 1. Understand the Constraints

Before diving into the solution, make sure you understand the problem's constraints. The lengths of the strings will not be more than 100 for `s1` and `s2`, and not more than 200 for `s3`. This can help you gauge the time complexity you should aim for.

### 2. Multiple Approaches

There are multiple ways to solve this problem, including:

- 2D Dynamic Programming
- 1D Dynamic Programming
- Recursion with Memoization

Each method has its own time and space complexity, so choose based on the problem's constraints.

### 3. Space Optimization

While 2D Dynamic Programming is the most intuitive approach, you can reduce the space complexity to (O(\min(m, n))) by employing 1D Dynamic Programming. In an interview setting, discussing this optimization can impress your interviewer.

### 4. Early Exit

If the sum of the lengths of `s1` and `s2` does not match the length of `s3`, you can immediately return `false`. This can save computation time and demonstrate that you're mindful of edge cases.

# Approach: 2D Dynamic Programming

To solve the "Interleaving String" problem using 2D Dynamic Programming, we utilize a 2D array `dp[i][j]` to represent whether the substring `s3[:i+j]` can be formed by interleaving `s1[:i]` and `s2[:j]`.

## Key Data Structures:

- **dp**: A 2D list to store the results of subproblems.

## Enhanced Breakdown:

1. **Initialization**:
    
    - Calculate lengths of `s1`, `s2`, and `s3`.
    - If the sum of lengths of `s1` and `s2` is not equal to the length of `s3`, return false.
    - Initialize the `dp` array with dimensions `(m+1) x (n+1)`, setting `dp[0][0] = True`.
2. **Base Cases**:
    
    - Fill in the first row of `dp` array, considering only the characters from `s1`.
    - Fill in the first column of `dp` array, considering only the characters from `s2`.
3. **DP Loop**:
    
    - Loop through each possible `(i, j)` combination, starting from `(1, 1)`.
    - Update `dp[i][j]` based on the transition `dp[i][j] = (dp[i-1][j] and s1[i-1] == s3[i+j-1]) or (dp[i][j-1] and s2[j-1] == s3[i+j-1])`.
4. **Wrap-up**:
    
    - Return the value stored in `dp[m][n]`, which indicates whether `s3` can be formed by interleaving `s1` and `s2`.
## CODE

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size(), l = s3.size();

        if (m + n != l) {
            return false;
        }

        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        dp[0][0] = true;

        for (int i = 1; i <= m; i++) {
            dp[i][0] = dp[i - 1][0] && s1[i - 1] == s3[i - 1];
        }

        for (int j = 1; j <= n; j++) {
            dp[0][j] = dp[0][j - 1] && s2[j - 1] == s3[j - 1];
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] =
                    (dp[i - 1][j] && s1[i - 1] == s3[i + j - 1]) ||
                    (dp[i][j - 1] && s2[j - 1] == s3[i + j - 1]);
            }
        }

        return dp[m][n];
    }
};
```
## Complexity:

**Time Complexity:**

- The solution iterates over each possible (i,j) combination, leading to a time complexity of O(m×n).

**Space Complexity:**

- The space complexity is O(m×n) due to the 2D dp array.

---
# Approach: 1D Dynamic Programming

The optimization from 2D to 1D DP is based on the observation that the state of `dp[i][j]` in the 2D DP array depends only on `dp[i-1][j]` and `dp[i][j-1]`. Therefore, while iterating through the strings, the current state only depends on the states in the previous row of the 2D DP array, which means we can optimize our space complexity by just keeping track of one row (1D DP).

## Key Data Structures:

- **dp**: A 1D list that stores whether the substring `s3[:i+j]` can be formed by interleaving `s1[:i]` and `s2[:j]`. Initially, all values are set to `False` except `dp[0]`, which is set to `True`.

## Enhanced Breakdown:

1. **Initialization**:
    
    - First, calculate the lengths of `s1`, `s2`, and `s3`.
    - Check if the sum of the lengths of `s1` and `s2` equals the length of `s3`. If it doesn't, return `False` as `s3` cannot be formed by interleaving `s1` and `s2`.
2. **Optimization Check**:
    
    - If `m < n`, swap `s1` and `s2`. This is to ensure that `s1` is not longer than `s2`, which helps in optimizing the space complexity to `O(min(m, n))`.
3. **Base Cases**:
    
    - Initialize a 1D array `dp` of length `n+1` with `False`.
    - Set `dp[0] = True` because an empty `s1` and `s2` can interleave to form an empty `s3`.
4. **Single-Row DP Transition**:
    
    - Iterate through `s1` and `s2` to update the `dp` array.
    - For each character in `s1`, iterate through `s2` and update the `dp` array based on the transition rule: `dp[j] = (dp[j] and s1[i] == s3[i+j]) or (dp[j-1] and s2[j] == s3[i+j])`.
    - The transition rule checks if the current `s3[i+j]` can be matched by either `s1[i]` or `s2[j]`, relying solely on the previous values in the `dp` array.
5. **Wrap-up**:
    
    - The final value in the `dp` array will indicate whether the entire `s3` can be formed by interleaving `s1` and `s2`.
    - Return `dp[n]`.
## CODE

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.length(), n = s2.length(), l = s3.length();
        if (m + n != l) return false;
        
        if (m < n) return isInterleave(s2, s1, s3);

        vector<bool> dp(n + 1, false);
        dp[0] = true;

        for (int j = 1; j <= n; ++j) {
            dp[j] = dp[j - 1] && s2[j - 1] == s3[j - 1];
        }

        for (int i = 1; i <= m; ++i) {
            dp[0] = dp[0] && s1[i - 1] == s3[i - 1];
            for (int j = 1; j <= n; ++j) {
                dp[j] = (dp[j] && s1[i - 1] == s3[i + j - 1]) || (dp[j - 1] && s2[j - 1] == s3[i + j - 1]);
            }
        }
        
        return dp[n];
    }
};
```
## Complexity:

The primary advantage of this 1D DP approach is its space efficiency. While it maintains the same time complexity as the 2D DP approach O(m×n), the space complexity is optimized to O(min(m,n)).

**Time Complexity:**

- The solution iterates over each character of `s1` and `s2` once, leading to a complexity of O(m×n).

**Space Complexity:**

- The space complexity is optimized to O(min(m,n)) as we're only using a single 1D array instead of a 2D matrix.

---

# Approach: Recursion with Memoization

In this approach, we recursively check whether the substring `s3[k:]` can be formed by interleaving `s1[i:]` and `s2[j:]`. We store the results of these sub-problems in a dictionary named `memo`.

## Key Data Structures:

- **memo**: A dictionary to store the results of subproblems.

## Enhanced Breakdown:

1. **Initialization**:
    
    - Calculate lengths of `s1`, `s2`, and `s3`.
    - If the sum of lengths of `s1` and `s2` is not equal to the length of `s3`, return false.
2. **Recursive Function**:
    
    - Define a recursive function `helper` which takes indices `i`, `j`, and `k` as inputs.
    - The function checks whether the substring `s3[k:]` can be formed by interleaving `s1[i:]` and `s2[j:]`.
    - Store the result of each subproblem in the `memo` dictionary.
3. **Wrap-up**:
    
    - Return the result of the recursive function for the initial values `i=0, j=0, k=0`.
## CODE

```cpp
class Solution {
public:
    bool isInterleave(string s1, string s2, string s3) {
        int m = s1.size(), n = s2.size(), l = s3.size();

        if (m + n != l) {
            return false;
        }

        map<pair<int, int>, bool> memo;

        function<bool(int, int, int)> helper = [&](int i, int j, int k) -> bool {
            if (k == l) {
                return true;
            }

            if (memo.count({i, j})) {
                return memo[{i, j}];
            }

            bool ans = false;

            if (i < m && s1[i] == s3[k]) {
                ans = ans || helper(i + 1, j, k + 1);
            }

            if (j < n && s2[j] == s3[k]) {
                ans = ans || helper(i, j + 1, k + 1);
            }

            memo[{i, j}] = ans;
            return ans;
        };

        return helper(0, 0, 0);
    }
};
```
## Complexity:

**Time Complexity:**

- Each combination of (i, j) is computed once and stored in the memo, leading to a time complexity of O(m×n).

**Space Complexity:**

- The space complexity is O(m×n) for storing the memoization results.