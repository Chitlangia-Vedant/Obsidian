# [322. Coin Change](https://leetcode.com/problems/coin-change/)

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return _the fewest number of coins that you need to make up that amount_. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

**Example 1:**

**Input:** coins = [1,2,5], amount = 11
**Output:** 3
**Explanation:** 11 = 5 + 5 + 1

**Example 2:**

**Input:** coins = [2], amount = 3
**Output:** -1

**Example 3:**

**Input:** coins = [1], amount = 0
**Output:** 0

**Constraints:**

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`
  
```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        
    }
};
```

## (1) Identify:
"Return the fewest number of coins"
"Return the minimum number of coins"
# Solution:

## (1) Brute Force: Recursion

Permutations: (Number of Coins ^ Amount)

```Java
class Solution {
public int coinChange(int[] coins, int amount) 
{
	if (amount < 0)
		return -1;

	if (amount == 0)
		return 0;

	int ans = -1;
	int n = coins.length;
	for (int i=0; i<n; i++)
	{
        // Recur for every value in array to check if amount becomes 0
		int val = coinChange(coins, amount-coins[i]);
		
        // val < 0: Means cannot make
        // Update the min for all values >= 0
		if (val >=0)
			ans = ans < 0 ? val + 1 : Math.min(ans, val +1);
	}

	return ans;        
}

}
```

TC: O(branches ^ depth)
branches = Array Size = coins[]
depth = amount
TC: O(coins.length ^ amount)

SC: O(amount) - Recursion Stack

15/189 TC Passed

## (2) Optimised: DP

```Java
// Store in DP:
// dp[val] = Min Coin Counts

// How to calculate DP?
// dp[i] =  Math.min(ans, val +1)
// dp[i] = Math.min(dp[i], dp[i-coins[j]] + 1);
class Solution {
    public int coinChange(int[] coins, int amount) 
{
    if (amount < 0)
		return -1;

	if (amount == 0)
		return 0;

    int[] dp = new int[amount+1]; // dp[val] = Min Coin Counts
    for (int i=1; i<dp.length; i++)
    {
        dp[i] = dp.length;
        for (int j=0; j<coins.length; j++)
        {
            if (i >= coins[j])
            dp[i] = Math.min(dp[i], dp[i-coins[j]] + 1);
        }
        //  dp[i-coins[j]] = amount-coins[i] in Recursion
    }

    if (dp[amount] == dp.length)
        return -1;
    
    return dp[amount];
}
}
```