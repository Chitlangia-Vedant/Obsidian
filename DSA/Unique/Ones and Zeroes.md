# [474. Ones and Zeroes](https://leetcode.com/problems/ones-and-zeroes/)

You are given an array of binary strings `strs` and two integers `m` and `n`.

Return _the size of the largest subset of `strs` such that there are **at most**_ `m` `0`_'s and_ `n` `1`_'s in the subset_.

A set `x` is a **subset** of a set `y` if all elements of `x` are also elements of `y`.

**Example 1:**

**Input:** strs = ["10","0001","111001","1","0"], m = 5, n = 3
**Output:** 4
**Explanation:** The largest subset with at most 5 0's and 3 1's is {"10", "0001", "1", "0"}, so the answer is 4.
Other valid but smaller subsets include {"0001", "1"} and {"10", "1", "0"}.
{"111001"} is an invalid subset because it contains 4 1's, greater than the maximum of 3.

**Example 2:**

**Input:** strs = ["10","0","1"], m = 1, n = 1
**Output:** 2
**Explanation:** The largest subset is {"0", "1"}, so the answer is 2.

**Constraints:**

- `1 <= strs.length <= 600`
- `1 <= strs[i].length <= 100`
- `strs[i]` consists only of digits `'0'` and `'1'`.
- `1 <= m, n <= 100`
# Solution
## 1. Recursion

### Intuition

This problem is a variant of the 0/1 knapsack problem with two constraints instead of one. For each binary string, we must decide whether to include it in our subset or not.

We can try all possible combinations by exploring two branches at each string: include it (if we have enough zeros and ones remaining) or skip it. The goal is to maximize the count of strings we can include while staying within the budget of `m` zeros and `n` ones.

### Algorithm

1. Preprocess each string to count its zeros and ones, storing in an array `arr`.
2. Define a recursive function `dfs(i, m, n)` that returns the maximum strings we can select starting from index `i` with `m` zeros and `n` ones remaining.
3. Base case: If `i` reaches the end of the array, return `0`.
4. At each index, we have two choices:
    - Skip the current string: `dfs(i + 1, m, n)`.
    - Include the current string (if affordable): `1 + dfs(i + 1, m - zeros, n - ones)`.
5. Return the maximum of both choices.

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> arr(strs.size(), vector<int>(2));
        for (int i = 0; i < strs.size(); i++) {
            for (char c : strs[i]) {
                arr[i][c - '0']++;
            }
        }
        return dfs(0, m, n, arr);
    }

private:
    int dfs(int i, int m, int n, vector<vector<int>>& arr) {
        if (i == arr.size()) {
            return 0;
        }

        int res = dfs(i + 1, m, n, arr);
        if (m >= arr[i][0] && n >= arr[i][1]) {
            res = max(res, 1 + dfs(i + 1, m - arr[i][0], n - arr[i][1], arr));
        }
        return res;
    }
};
```

### Time & Space Complexity

- Time complexity: O(2N)
- Space complexity: O(N) for recursion stack.

> Where NN represents the number of binary strings, and mm and nn are the maximum allowable counts of zeros and ones, respectively.

---

## 2. Dynamic Programming (Top-Down)

### Intuition

The recursive solution has overlapping subproblems. The same state `(i, m, n)` can be reached through different paths, leading to redundant computations.

We add memoization to cache results for each unique state. The state is defined by three variables: the current string index, remaining zeros budget, and remaining ones budget. Once we compute the answer for a state, we store it and return immediately on future calls.

### Algorithm

1. Preprocess each string to count its zeros and ones.
2. Create a 3D memoization table indexed by `(i, m, n)`.
3. Define `dfs(i, m, n)` as before, but check the cache first and store results before returning.
4. Early termination: If both `m` and `n` are `0`, we cannot include any more strings.
5. Return `dfs(0, m, n)`.

```cpp
class Solution {
private:
    vector<vector<vector<int>>> dp;
    vector<vector<int>> arr;

public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        arr = vector<vector<int>>(strs.size(), vector<int>(2));
        for (int i = 0; i < strs.size(); i++) {
            for (char c : strs[i]) {
                arr[i][c - '0']++;
            }
        }

        dp = vector<vector<vector<int>>>(strs.size(), vector<vector<int>>(m + 1, vector<int>(n + 1, -1)));
        return dfs(0, m, n);
    }

private:
    int dfs(int i, int m, int n) {
        if (i == arr.size()) {
            return 0;
        }
        if (m == 0 && n == 0) {
            return 0;
        }
        if (dp[i][m][n] != -1) {
            return dp[i][m][n];
        }

        int res = dfs(i + 1, m, n);
        if (m >= arr[i][0] && n >= arr[i][1]) {
            res = max(res, 1 + dfs(i + 1, m - arr[i][0], n - arr[i][1]));
        }
        dp[i][m][n] = res;
        return res;
    }
};
```

### Time & Space Complexity

- Time complexity: O(m∗n∗N)
- Space complexity: O(m∗n∗N)

> Where NN represents the number of binary strings, and mm and nn are the maximum allowable counts of zeros and ones, respectively.

---

## 3. Dynamic Programming (Bottom-Up)

### Intuition

We can convert the top-down approach to bottom-up by building the solution iteratively. We process strings one by one and, for each combination of remaining zeros and ones budget, compute the maximum strings achievable.

The DP table `dp[i][j][k]` represents the maximum strings from the first `i` strings using at most `j` zeros and `k` ones.

### Algorithm

1. Preprocess each string to count its zeros and ones.
2. Create a 3D DP table of size `(len(strs) + 1) x (m + 1) x (n + 1)`, initialized to `0`.
3. For each string `i` from `1` to `len(strs)`:
    - For each zeros budget `j` from `0` to `m`:
        - For each ones budget `k` from `0` to `n`:
            - Copy the value from the previous string: `dp[i][j][k] = dp[i-1][j][k]`.
            - If we can afford the current string (`j >= zeros` and `k >= ones`):
                - Update: `dp[i][j][k] = max(dp[i][j][k], 1 + dp[i-1][j-zeros][k-ones])`.
4. Return `dp[len(strs)][m][n]`.

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> arr(strs.size(), vector<int>(2));
        for (int i = 0; i < strs.size(); i++) {
            for (char c : strs[i]) {
                arr[i][c - '0']++;
            }
        }

        vector<vector<vector<int>>> dp(strs.size() + 1, vector<vector<int>>(m + 1, vector<int>(n + 1, 0)));

        for (int i = 1; i <= strs.size(); i++) {
            for (int j = 0; j <= m; j++) {
                for (int k = 0; k <= n; k++) {
                    dp[i][j][k] = dp[i - 1][j][k];
                    if (j >= arr[i - 1][0] && k >= arr[i - 1][1]) {
                        dp[i][j][k] = max(dp[i][j][k], 1 + dp[i - 1][j - arr[i - 1][0]][k - arr[i - 1][1]]);
                    }
                }
            }
        }

        return dp[strs.size()][m][n];
    }
};
```

### Time & Space Complexity

- Time complexity: O(m∗n∗N)
- Space complexity: O(m∗n∗N)

> Where NN represents the number of binary strings, and mm and nn are the maximum allowable counts of zeros and ones, respectively.

---

## 4. Dynamic Programming (Space Optimized)

### Intuition

Notice that when computing `dp[i]`, we only need values from `dp[i-1]`. This means we can reduce the 3D table to a 2D table.

The key trick is to iterate the budgets in reverse order. When we update `dp[j][k]`, we need the old values of `dp[j-zeros][k-ones]`. By iterating backward, we ensure these values have not been overwritten yet in the current iteration.

### Algorithm

1. Preprocess each string to count its zeros and ones.
2. Create a 2D DP table of size `(m + 1) x (n + 1)`, initialized to `0`.
3. For each string with `zeros` zeros and `ones` ones:
    - For `j` from `m` down to `zeros`:
        - For `k` from `n` down to `ones`:
            - Update: `dp[j][k] = max(dp[j][k], 1 + dp[j-zeros][k-ones])`.
4. Return `dp[m][n]`.

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        vector<vector<int>> arr(strs.size(), vector<int>(2));
        for (int i = 0; i < strs.size(); i++) {
            for (char c : strs[i]) {
                arr[i][c - '0']++;
            }
        }

        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

        for (const auto& pair : arr) {
            int zeros = pair[0], ones = pair[1];
            for (int j = m; j >= zeros; j--) {
                for (int k = n; k >= ones; k--) {
                    dp[j][k] = max(dp[j][k], 1 + dp[j - zeros][k - ones]);
                }
            }
        }

        return dp[m][n];
    }
};
```

### Time & Space Complexity

- Time complexity: O(m∗n∗N)
- Space complexity: O(m∗n+N)

> Where N represents the number of binary strings, and m and n are the maximum allowable counts of zeros and ones, respectively.