# [1137. N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/)

The Tribonacci sequence Tn is defined as follows: 

T0 = 0, T1 = 1, T2 = 1, and Tn+3 = Tn + Tn+1 + Tn+2 for n >= 0.

Given `n`, return the value of Tn.

**Example 1:**

**Input:** n = 4
**Output:** 4
**Explanation:**
T_3 = 0 + 1 + 1 = 2
T_4 = 1 + 1 + 2 = 4

**Example 2:**

**Input:** n = 25
**Output:** 1389537

**Constraints:**

- `0 <= n <= 37`
- The answer is guaranteed to fit within a 32-bit integer, ie. `answer <= 2^31 - 1`.

```cpp
class Solution {
public:
    int tribonacci(int n) {
        
    }
};
```

# Solution:

## (1) Recursive Solution:

TC: O(3^N)
SC: O(N) - Auxiliary Space
## (2) Bottom Up DP: Tabulation

TC: O(N)
SC: O(N)
## (3) Top Down DP: Memoisation

TC: O(N)
SC: O(N)

# CODE:

Bottom Up DP: Tabulation (0 to N)

```java
class Solution 
{
    public int tribonacci(int n) 
    {
        if (n<=1)
            return n;

        int[] dp = new int[n+1];
        // Base Case:
        dp[0] = 0;
        dp[1] = 1;
        dp[2] = 1;

        for (int i = 3; i<=n; i++)
            dp[i] = dp[i-1] + dp[i-2] + dp[i-3];

        return dp[n];
    }
}
```

TC: O(N)
SC: O(N)
# CODE:

Top Down DP: Memoisation (N to 0)

Recur on Lower States (i-1, i-2, i-3): DFS Approach

```java
class Solution 
{
    public int tribonacci(int n) 
    {
        int[] dp = new int[n+1];
        return dfs(n, dp); // dfs     
    }

    int dfs(int n, int[] dp)
    {
        if (n<=1)
            return n;

        if (n==2)
            return 1;

        // If Not Empty, already Contained Answer, return ans
        if(dp[n]!=0)
            return dp[n];

        // If Empty, Not Yet Calculated ----> Calculate
        return dp[n] = dfs(n-1, dp) + dfs(n-2, dp) + dfs(n-3, dp);
    }
}
```

TC: O(N)
SC: O(N)