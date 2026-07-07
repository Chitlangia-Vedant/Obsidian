[1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

Given two strings `text1` and `text2`, return _the length of their longest **common subsequence**._ If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

**Example 1:**

**Input:** text1 = "abcde", text2 = "ace" 
**Output:** 3  
**Explanation:** The longest common subsequence is "ace" and its length is 3.

**Example 2:**

**Input:** text1 = "abc", text2 = "abc"
**Output:** 3
**Explanation:** The longest common subsequence is "abc" and its length is 3.

**Example 3:**

**Input:** text1 = "abc", text2 = "def"
**Output:** 0
**Explanation:** There is no such common subsequence, so the result is 0.

**Constraints:**

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        
    }
};
```

# Solution: Top Down DP

- Store the two input strings `text1` and `text2` in global variables `t1` and `t2`.
- Create a 2D DP table `dp` of size `text1.length() × text2.length()` and initialize all entries to `-1`.
- Call the recursive function `solve(text1.length() - 1, text2.length() - 1)` to compute the length of the Longest Common Subsequence (LCS).
- In `solve(i, j)`, first check whether `dp[i][j]` has already been computed. If so, return its value.
- If both `i == 0` and `j == 0`, compare the first characters of the two strings. Store `1` in `dp[0][0]` if they match; otherwise, store `0`. Return the stored value.
- If `i == 0`, check whether the first character of `t1` appears anywhere in the prefix `t2[0...j]`. If it does, store `1` in `dp[0][j]`; otherwise, store `0`. Return the stored value.
- If `j == 0`, check whether the first character of `t2` appears anywhere in the prefix `t1[0...i]`. If it does, store `1` in `dp[i][0]`; otherwise, store `0`. Return the stored value.
- If `t1[i]` and `t2[j]` are equal, recursively compute the LCS of the prefixes ending at `i-1` and `j-1`, add `1`, and store the result in `dp[i][j]`.
- Otherwise, recursively compute the LCS by excluding the current character from either `t1` or `t2`, take the maximum of `solve(i-1, j)` and `solve(i, j-1)`, and store it in `dp[i][j]`.
- Return the value stored in `dp[i][j]`.
- After the recursive computation finishes, return the value obtained from `solve(text1.length() - 1, text2.length() - 1)` as the length of the Longest Common Subsequence
## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> dp;
    string t1;
    string t2;
    int longestCommonSubsequence(string text1, string text2) {
        dp.assign(text1.length(),vector<int>(text2.length(),-1));
        t1 = text1;
        t2 = text2;
        return solve(text1.length()-1,text2.length()-1);
    }
    int solve(int i,int j){
        //cout<<i<<" "<<j<<"\n";
        if(dp[i][j]!=-1) return dp[i][j];
        if(i==0&&j==0){
            dp[i][j]=t1[0]==t2[0]?1:0;
            return dp[i][j];
        }
        if(i==0){
            dp[i][j]=t2.substr(0,j+1).contains(t1[0])?1:0;
            return dp[i][j];
        }
        if(j==0){
            dp[i][j]=t1.substr(0,i+1).contains(t2[0])?1:0;
            return dp[i][j];
        }
        dp[i][j]=t1[i]==t2[j]?solve(i-1,j-1)+1:max(solve(i-1,j),solve(i,j-1));
        return dp[i][j];
    }
};
```

# Solution: Bottom Up DP

## Intuition:

The problem asks for finding the length of the longest common subsequence between two strings. A common subsequence is a sequence of characters that appear in the same order in both strings. The dynamic programming approach can be used to efficiently find the length of the longest common subsequence.

## Approach:

1. **Dynamic Programming (DP):**
    
    - We create a 2D array `dp` to store the lengths of the longest common subsequences for all subproblems.
    - The array `dp` has dimensions `(length1 + 1) x (length2 + 1)` to accommodate empty strings as substrings.
    - The cell `dp[i][j]` represents the length of the longest common subsequence of substrings `text1[0...i-1]` and `text2[0...j-1]`.
    - We fill in the `dp` array from the bottom up using the following recurrence relation:
        - If characters at indices `i-1` and `j-1` match, `dp[i][j] = dp[i-1][j-1] + 1`.
        - If characters do not match, `dp[i][j] = max(dp[i-1][j], dp[i][j-1])`.
2. **Result:**
    
    - The bottom-right cell `dp[length1][length2]` contains the length of the longest common subsequence of the entire strings `text1` and `text2`.

## Complexity Analysis:

- **Time Complexity:** O(length1 * length2)
    - The nested loops iterate through each character in both strings, filling the 2D array.
- **Space Complexity:** O(length1 * length2)
    - The space used by the 2D array to store intermediate results.

## Code Explanation:

- The code initializes a 2D array `dp` and iterates through both strings, updating the `dp` array according to the recurrence relation.
- The result is obtained from the bottom-right cell of the `dp` array, which represents the length of the longest common subsequence.

## Code

```cpp
class Solution {
public:
    // Function to find the length of the longest common subsequence in two strings.
    int longestCommonSubsequence(string text1, string text2) {
        int text1Length = text1.size(), text2Length = text2.size();
        // Create a 2D array to store lengths of common subsequence at each index.
        int dp[text1Length + 1][text2Length + 1];
      
        // Initialize the 2D array with zero.
        memset(dp, 0, sizeof dp);
      
        // Loop through both strings and fill the dp array.
        for (int i = 1; i <= text1Length; ++i) {
            for (int j = 1; j <= text2Length; ++j) {
                // If current characters match, add 1 to the length of the sequence
                // until the previous character from both strings.
                if (text1[i - 1] == text2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    // If current characters do not match, take the maximum length
                    // achieved by either skipping the current character of text1 or text2.
                    dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }
      
        // Return the value in the bottom-right cell which contains the
        // length of the longest common subsequence for the entire strings.
        return dp[text1Length][text2Length];
    }
};
```