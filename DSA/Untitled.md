[790. Domino and Tromino Tiling](https://leetcode.com/problems/domino-and-tromino-tiling/)

You have two types of tiles: a `2 x 1` domino shape and a tromino shape. You may rotate these shapes.

![](https://assets.leetcode.com/uploads/2021/07/15/lc-domino.jpg)

Given an integer n, return _the number of ways to tile an_ `2 x n` _board_. Since the answer may be very large, return it **modulo** `109 + 7`.

In a tiling, every square must be covered by a tile. Two tilings are different if and only if there are two 4-directionally adjacent cells on the board such that exactly one of the tilings has both squares occupied by a tile.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/07/15/lc-domino1.jpg)

**Input:** n = 3
**Output:** 5
**Explanation:** The five different ways are shown above.

**Example 2:**

**Input:** n = 1
**Output:** 1

**Constraints:**

- `1 <= n <= 1000`

# Solution 

- dp[i] denotes the number of ways to tile an 2 * (i + 1) board, note that dp is 0-indexed.
    - Intuitively, dp[0] = 1 and dp[1] = 2

![[Pasted image 20260714134934.png]]
- dpa[i] denotes the number of ways to tile an 2 * i board and 1 more square left below(or above symmetrically).
    - Intuitively, dpa[0] = 0 and dpa[1] = 1
    - I just explained the case where in i-th column, 2nd row is filled. But it should be noted that the two cases(the other is in i-th column, 1st row is filled) are symmetric and the numbers are both dpa[i], you may imagine dpb[i] = dpa[i] for the second case where i-th column 1st row is filled. 
 
![[Pasted image 20260714134949.png]]

Further More!
![[Pasted image 20260714135112.png]]

# CODE
```cpp
class Solution {
public:
int numTilings(int n) {
    unsigned int dp[n + 3]; dp[0] = 1; dp[1] = 2; dp[2] = 5;
    for (int i = 3; i < n; i ++) {
        dp[i] = (2 * dp[i - 1] + dp[i - 3]) % 1000000007;
    }
    return dp[n - 1];
}
};
```