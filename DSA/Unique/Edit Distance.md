[72. Edit Distance](https://leetcode.com/problems/edit-distance/)

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"
**Output:** 3
**Explanation:** 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"
**Output:** 5
**Explanation:** 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

**Constraints:**

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

# Solution

## 1. Recursion

### Intuition

This problem asks for the **minimum number of operations** required to convert `word1` into `word2`.  
The allowed operations are:

- insert a character
- delete a character
- replace a character

At any position in both strings, we compare characters at indices `i` and `j`.

The recursive function represents:  
**"What is the minimum number of operations needed to convert `word1[i:]` into `word2[j:]`?"**

If the characters already match, we simply move forward in both strings without using any operation.  
If they do not match, we try all three possible operations and take the minimum cost.

### Algorithm

1. Let `m = len(word1)` and `n = len(word2)`.
2. Define a recursive function `dfs(i, j)`:
    - `i` is the current index in `word1`
    - `j` is the current index in `word2`
3. If we reach the end of `word1`:
    - All remaining characters in `word2` must be inserted
    - Return `n - j`
4. If we reach the end of `word2`:
    - All remaining characters in `word1` must be deleted
    - Return `m - i`
5. If `word1[i] == word2[j]`:
    - No operation is needed
    - Move both pointers forward: `dfs(i + 1, j + 1)`
6. Otherwise, consider all three operations:
    - **Delete** from `word1`: `dfs(i + 1, j)`
    - **Insert** into `word1`: `dfs(i, j + 1)`
    - **Replace** the character: `dfs(i + 1, j + 1)`
7. Take the minimum of these three results and add `1` for the current operation
8. Start the recursion from `(0, 0)` and return the final result
### CODE
Edit Distance - Dynamic Programming (Recursion)

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        return dfs(0, 0, word1, word2, m, n);
    }

    int dfs(int i, int j, string& word1, string& word2, int m, int n) {
        if (i == m) return n - j;
        if (j == n) return m - i;
        if (word1[i] == word2[j]){
            return dfs(i + 1, j + 1, word1, word2, m, n);
        }

        int res = min(dfs(i + 1, j, word1, word2, m, n),
                      dfs(i, j + 1, word1, word2, m, n));
        res = min(res, dfs(i + 1, j + 1, word1, word2, m, n));
        return res + 1;
    }
};
```

### Time & Space Complexity

- Time complexity: O(3m+n)
- Space complexity: O(m+n)

> Where mm is the length of word1word1 and nn is the length of word2word2.

---

## 2. Dynamic Programming (Top-Down)

### Intuition

This problem asks for the **minimum number of edit operations** required to convert `word1` into `word2`.  
The allowed operations are:

- insert a character
- delete a character
- replace a character

The recursive solution explores all possibilities, but many subproblems repeat. To optimize this, we use **top-down dynamic programming (memoization)**.

A state is uniquely defined by:

- `i`: current index in `word1`
- `j`: current index in `word2`

The recursive function answers:  
**"What is the minimum number of operations needed to convert `word1[i:]` into `word2[j:]`?"**

By caching results for each `(i, j)` pair, we avoid recomputing the same states.

### Algorithm

1. Let `m = len(word1)` and `n = len(word2)`.
2. Create a memoization map `dp` where:
    - `dp[(i, j)]` stores the minimum edit distance for `word1[i:]` and `word2[j:]`
3. Define a recursive function `dfs(i, j)`:
    - `i` is the current index in `word1`
    - `j` is the current index in `word2`
4. If `i` reaches the end of `word1`:
    - Return the number of remaining characters in `word2` (`n - j`)
5. If `j` reaches the end of `word2`:
    - Return the number of remaining characters in `word1` (`m - i`)
6. If the state `(i, j)` is already in `dp`:
    - Return the cached result
7. If `word1[i] == word2[j]`:
    - No operation is needed
    - Store and return `dfs(i + 1, j + 1)`
8. Otherwise, consider all three operations:
    - Delete from `word1` → `dfs(i + 1, j)`
    - Insert into `word1` → `dfs(i, j + 1)`
    - Replace the character → `dfs(i + 1, j + 1)`
9. Take the minimum of the three results, add `1`, and store it in `dp[(i, j)]`
10. Start the recursion from `(0, 0)` and return the final answer

### CODE
Edit Distance - Dynamic Programming with Memoization

```cpp
class Solution {
    vector<vector<int>> dp;
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        dp = vector<vector<int>>(m, vector<int>(n, -1));
        return dfs(0, 0, word1, word2, m, n);
    }

    int dfs(int i, int j, string& word1, string& word2, int m, int n) {
        if (i == m) return n - j;
        if (j == n) return m - i;
        if (dp[i][j] != -1) return dp[i][j];
        if (word1[i] == word2[j]){
            dp[i][j] = dfs(i + 1, j + 1, word1, word2, m, n);
        } else {
            int res = min(dfs(i + 1, j, word1, word2, m, n),
                        dfs(i, j + 1, word1, word2, m, n));
            res = min(res, dfs(i + 1, j + 1, word1, word2, m, n));
            dp[i][j] = res + 1;
        }
        return dp[i][j];
    }
};
```
### Time & Space Complexity

- Time complexity: O(m∗n)
- Space complexity: O(m∗n)

> Where mm is the length of word1word1 and nn is the length of word2word2.

---

## 3. Dynamic Programming (Bottom-Up)

### Intuition

We want the **minimum number of edits** needed to convert `word1` into `word2`, where an edit can be:

- insert a character
- delete a character
- replace a character

Instead of using recursion, we can solve this using **bottom-up dynamic programming** by building the answer for smaller suffixes first.

We define a DP state that answers:  
**"What is the minimum edit distance between `word1[i:]` and `word2[j:]`?"**

By filling a table from the end of the strings toward the beginning, every subproblem we need is already solved when we reach it.

### Algorithm

1. Create a 2D DP table `dp` of size  
    `(len(word1) + 1) × (len(word2) + 1)`.
2. Let `dp[i][j]` represent the minimum number of operations to convert  
    `word1[i:]` into `word2[j:]`.
3. Initialize the base cases:
    - If `word1` is exhausted (`i == len(word1)`), all remaining characters of `word2` must be inserted:
        - `dp[len(word1)][j] = len(word2) - j`
    - If `word2` is exhausted (`j == len(word2)`), all remaining characters of `word1` must be deleted:
        - `dp[i][len(word2)] = len(word1) - i`
4. Fill the DP table from bottom-right to top-left:
5. For each position `(i, j)`:
    - If `word1[i] == word2[j]`:
        - No operation is needed
        - `dp[i][j] = dp[i + 1][j + 1]`
    - Otherwise:
        - Consider all three operations:
            - Delete → `dp[i + 1][j]`
            - Insert → `dp[i][j + 1]`
            - Replace → `dp[i + 1][j + 1]`
        - Take the minimum of these and add `1`
6. After filling the table, the answer is stored in `dp[0][0]`
7. Return `dp[0][0]`

### CODE
Edit Distance - Dynamic Programming

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        vector<vector<int>> dp(word1.length() + 1,
                               vector<int>(word2.length() + 1, 0));

        for (int j = 0; j <= word2.length(); j++) {
            dp[word1.length()][j] = word2.length() - j;
        }
        for (int i = 0; i <= word1.length(); i++) {
            dp[i][word2.length()] = word1.length() - i;
        }

        for (int i = word1.length() - 1; i >= 0; i--) {
            for (int j = word2.length() - 1; j >= 0; j--) {
                if (word1[i] == word2[j]) {
                    dp[i][j] = dp[i + 1][j + 1];
                } else {
                    dp[i][j] = 1 + min(dp[i + 1][j],
                                   min(dp[i][j + 1], dp[i + 1][j + 1]));
                }
            }
        }
        return dp[0][0];
    }
};
```

### Time & Space Complexity

- Time complexity: O(m∗n)
- Space complexity: O(m∗n)

> Where mm is the length of word1word1 and nn is the length of word2word2.

---

## 4. Dynamic Programming (Space Optimized)

### Intuition

We want the minimum number of edits (insert, delete, replace) to convert `word1` into `word2`.

In the 2D DP solution, we used `dp[i][j]` to represent the answer for `word1[i:]` and `word2[j:]`.  
But notice that each cell `dp[i][j]` depends only on:

- `dp[i + 1][j]` (delete)
- `dp[i][j + 1]` (insert)
- `dp[i + 1][j + 1]` (replace / match)

So when filling the table from bottom to top, we only need:

- the **next row** (`i + 1`) and
- the **current row** being built (`i`)

That means we can optimize space by keeping just two 1D arrays:

- `dp` for the next row
- `nextDp` for the current row

To reduce memory even more, we also ensure the 1D arrays are based on the **shorter string** (swap if needed).

### Algorithm

1. Let `m = len(word1)` and `n = len(word2)`.
2. If `word2` is longer than `word1`, swap them so that `n` is the smaller length (this keeps the DP arrays small).
3. Create two arrays of size `n + 1`:
    - `dp` represents the DP values for row `i + 1`
    - `nextDp` represents the DP values for row `i`
4. Initialize the base case for when `word1` is exhausted:
    - `dp[j] = n - j` for all `j`
    - meaning if `word1` is empty, we must insert the remaining characters of `word2`
5. Iterate `i` from `m - 1` down to `0`:
    - Set `nextDp[n] = m - i`
    - meaning if `word2` is exhausted, we must delete the remaining characters of `word1`
6. For each `i`, iterate `j` from `n - 1` down to `0`:
    - If `word1[i] == word2[j]`:
        - no edit needed: `nextDp[j] = dp[j + 1]`
    - Otherwise:
        - take `1` + minimum of:
            - delete: `dp[j]`
            - insert: `nextDp[j + 1]`
            - replace: `dp[j + 1]`
7. After finishing the row, copy `nextDp` into `dp` for the next iteration.
8. The final answer is `dp[0]`, which represents converting `word1[0:]` to `word2[0:]`.
9. Return `dp[0]`
### CODE
Edit Distance - Dynamic Programming

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        if (m < n) {
            swap(m, n);
            swap(word1, word2);
        }

        vector<int> dp(n + 1), nextDp(n + 1);

        for (int j = 0; j <= n; ++j) {
            dp[j] = n - j;
        }

        for (int i = m - 1; i >= 0; --i) {
            nextDp[n] = m - i;
            for (int j = n - 1; j >= 0; --j) {
                if (word1[i] == word2[j]) {
                    nextDp[j] = dp[j + 1];
                } else {
                    nextDp[j] = 1 + min({dp[j], nextDp[j + 1], dp[j + 1]});
                }
            }
            dp = nextDp;
        }
        return dp[0];
    }
};
```
### Time & Space Complexity

- Time complexity: O(m∗n)
- Space complexity: O(min(m,n))

> Where mm is the length of word1word1 and nn is the length of word2word2.

---

## 5. Dynamic Programming (Optimal)

### Intuition

We want the minimum number of edits (insert, delete, replace) needed to convert `word1` into `word2`.

The classic DP idea is:

- `dp[i][j]` = minimum operations to convert `word1[i:]` into `word2[j:]`

But to compute `dp[i][j]`, we only need three neighboring states:

- `dp[i + 1][j]` (delete from `word1`)
- `dp[i][j + 1]` (insert into `word1`)
- `dp[i + 1][j + 1]` (replace, or match if characters are equal)

That means we don't need the full 2D table. We can compress it into a single 1D array `dp`, and update it row-by-row (from the end of the strings to the start).

The tricky part of in-place updates is that `dp[i + 1][j + 1]` (the diagonal value) would get overwritten.  
So we carry that diagonal value using one extra variable (`nextDp`), and another temporary variable to shift it correctly while moving left.

We also swap the strings if needed so the DP array is based on the shorter word, keeping memory minimal.

### Algorithm

1. Let `m = len(word1)` and `n = len(word2)`.
2. If `word2` is longer than `word1`, swap them so `n` is the smaller length (smaller DP array).
3. Create a 1D array `dp` of size `n + 1`:
    - Initialize it for the base case when `word1` is exhausted:
        - `dp[j] = n - j` (we must insert the remaining characters of `word2`)
4. Iterate `i` from `m - 1` down to `0`:
    - Store the old diagonal value using `nextDp` (this represents `dp[i + 1][j + 1]` during updates)
    - Update `dp[n] = m - i` (when `word2` is exhausted, we must delete remaining characters of `word1`)
5. Iterate `j` from `n - 1` down to `0`:
    - Save the current `dp[j]` in `temp` before overwriting it (this becomes the next diagonal)
    - If `word1[i] == word2[j]`:
        - no operation needed: set `dp[j] = nextDp`
    - Otherwise:
        - take `1` + minimum of:
            - delete: `dp[j]` (still represents `dp[i + 1][j]`)
            - insert: `dp[j + 1]` (already updated for current row)
            - replace: `nextDp` (the diagonal `dp[i + 1][j + 1]`)
    - Move the diagonal forward: set `nextDp = temp`
6. After finishing all updates, `dp[0]` represents converting the full `word1` into the full `word2`.
7. Return `dp[0]`
### CODE
```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        if (m < n) {
            swap(m, n);
            swap(word1, word2);
        }

        vector<int> dp(n + 1);
        for (int i = 0; i <= n; i++) dp[i] = n - i;

        for (int i = m - 1; i >= 0; i--) {
            int nextDp = dp[n];
            dp[n] = m - i;
            for (int j = n - 1; j >= 0; j--) {
                int temp = dp[j];
                if (word1[i] == word2[j]) {
                    dp[j] = nextDp;
                } else {
                    dp[j] = 1 + min({dp[j], dp[j + 1], nextDp});
                }
                nextDp = temp;
            }
        }
        return dp[0];
    }
};
```

### Time & Space Complexity

- Time complexity: O(m∗n)
- Space complexity: O(min(m,n))

> Where mm is the length of word1word1 and nn is the length of word2word2.

## Dynamic Programming (LCS + Edit Distance)

### Intuition

To convert `word1` into `word2`, we are allowed to:

- Insert a character
- Delete a character
- Replace a character

This solution first computes the **Longest Common Subsequence (LCS)** between the two strings.

The idea behind using LCS is that characters belonging to the LCS already appear in both strings in the correct relative order, so they do not need to be deleted or inserted. The `solve()` function computes the LCS length using memoization, while `build()` reconstructs every possible LCS by backtracking through the DP states.

However, instead of computing the answer directly from the LCS, the solution finally builds the classic **Edit Distance DP table**, where each state represents:

**"What is the minimum number of operations required to convert the first `i` characters of `word1` into the first `j` characters of `word2`?"**

Each state considers the three allowed operations:

- Replace the current character
- Delete a character from `word1`
- Insert a character into `word1`

The answer is stored in the last cell of the DP table.

> **Note:** Although the code computes and reconstructs all LCS sequences, those results are **not used** while computing the edit distance. The final answer depends entirely on the Edit Distance DP table, making the LCS computation unnecessary for this solution.

---

## Algorithm

1. Let `m` and `n` be the lengths of `word1` and `word2`.
2. Create a memoization table `dp` for computing the LCS.
3. Store both strings globally so they can be accessed by helper functions.
4. Call `build(m - 1, n - 1)`:
    - It uses the memoized LCS values from `solve()`.
    - It backtracks through the DP states to reconstruct every possible LCS.
    - The reconstructed sequences are stored in a set to avoid duplicates.
5. Create a DP table `editDp` of size `(m + 1) × (n + 1)`.
6. Initialize the base cases:
    - `editDp[i][0] = i` since converting a non-empty prefix into an empty string requires deleting all characters.
    - `editDp[0][j] = j` since converting an empty string into a non-empty prefix requires inserting all characters.
7. Fill the table row by row:
    - If the current characters are equal:
        - No operation is needed.
        - Copy the diagonal value:  
            `editDp[i][j] = editDp[i - 1][j - 1]`
    - Otherwise, consider all three possible operations:
        - Replace → `editDp[i - 1][j - 1]`
        - Delete → `editDp[i - 1][j]`
        - Insert → `editDp[i][j - 1]`
    - Take the minimum among them and add one for the current operation.
8. The final answer is stored in `editDp[m][n]`.
9. Return `editDp[m][n]`.

---

### Helper Function: `solve(i, j)`

`solve(i, j)` computes the **length of the Longest Common Subsequence** between:

- `word1[0...i]`
- `word2[0...j]`

It uses memoization to avoid recomputing overlapping subproblems.

- If either index becomes negative, no characters remain, so the LCS length is `0`.
- If the current characters match:
    - Include them in the LCS and move diagonally.
- Otherwise:
    - Skip one character from either string and take the larger LCS.

---

### Helper Function: `build(i, j)`

After the LCS lengths have been computed, `build()` reconstructs **all possible LCS sequences**.

- If characters match:
    - Add the matching indices to the current sequence.
    - Continue diagonally.
- Otherwise:
    - Compare the LCS values of the upper and left states.
    - Move toward the state with the larger LCS.
    - If both are equal, explore both paths.
- Whenever one string is exhausted:
    - Reverse the collected sequence.
    - Store it in a set to eliminate duplicates.

Although every LCS is reconstructed, these sequences are **never used** in the edit distance computation.
### CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> dp;
    string t1;
    string t2;
    set<vector<pair<int,int>>> ans;
    int minDistance(string word1, string word2) {
        int m=word1.length(),n=word2.length();
        dp.assign(m,vector<int>(n,-1));
        t1 = word1;
        t2 = word2;
        vector<pair<int,int>> cur;
        build(m - 1, n - 1, cur);
        vector<vector<int>> editDp(m + 1, vector<int>(n + 1));
        for (int i = 0; i <= m; i++) editDp[i][0] = i;
        for (int j = 0; j <= n; j++) editDp[0][j] = j;

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (t1[i - 1] == t2[j - 1]) {
                    editDp[i][j] = editDp[i - 1][j - 1];
                } else {
                    editDp[i][j] = 1 + min({editDp[i - 1][j - 1],
                                            editDp[i - 1][j],
                                            editDp[i][j - 1]});
                }
            }
        }

        int min_count = editDp[m][n];
        return min_count;
    }
    int solve(int i,int j){
        //cout<<i<<" "<<j<<"\n";
        if (i < 0 || j < 0)
            return 0;

        if (dp[i][j] != -1)
            return dp[i][j];

        if (t1[i] == t2[j])
            return dp[i][j] = 1 + solve(i - 1, j - 1);

        return dp[i][j] = max(solve(i - 1, j), solve(i, j - 1));
    }
    void build(int i, int j, vector<pair<int,int>> &cur) {
        if (i < 0 || j < 0) {
            reverse(cur.begin(), cur.end());
            ans.insert(cur);
            reverse(cur.begin(), cur.end());   // restore
            return;
        }

        if (t1[i] == t2[j]) {
            cur.push_back({i, j});
            build(i - 1, j - 1, cur);
            cur.pop_back();
        } else {
            int up = solve(i - 1, j);
            int left = solve(i, j - 1);

            if (up > left) {
                build(i - 1, j, cur);
            } else if (left > up) {
                build(i, j - 1, cur);
            } else {
                build(i - 1, j, cur);
                build(i, j - 1, cur);
            }
        }
    }
};
```

### Time & Space Complexity

- **Time complexity:** `O(m × n)`
- **Space complexity:** `O(m × n)`

> Where `m` is the length of `word1` and `n` is the length of `word2`.