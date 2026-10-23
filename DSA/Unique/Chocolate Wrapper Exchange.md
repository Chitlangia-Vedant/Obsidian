# Problem Statement

You are given `n` chocolate wrappers and an integer `m`. You can exchange exactly `m` wrappers for one additional chocolate. After eating that chocolate, you receive one wrapper back.

Return the maximum number of additional chocolates you can obtain through these exchanges.

# Examples

**Example 1:**

Input: `n = 5, m = 2`

Output: `4`

Explanation:
- Exchange 2 wrappers for 1 chocolate, leaving 4 wrappers.
- Exchange 2 wrappers for 1 chocolate, leaving 3 wrappers.
- Exchange 2 wrappers for 1 chocolate, leaving 2 wrappers.
- Exchange 2 wrappers for 1 chocolate, leaving 1 wrapper.

You can obtain 4 additional chocolates.

**Example 2:**

Input: `n = 10, m = 3`

Output: `4`

Explanation:
- Each exchange consumes 3 wrappers and returns 1 wrapper.
- Therefore, each exchange reduces the wrapper count by `m - 1 = 2`.
- The maximum number of exchanges is `(10 - 1) / (3 - 1) = 4`.

**Example 3:**

Input: `n = 15, m = 4`

Output: `4`

Explanation:
- Each exchange reduces the wrapper count by 3.
- The maximum number of exchanges is `(15 - 1) / (4 - 1) = 4`.

# Constraints

- `1 <= n <= 10^9`
- `2 <= m <= 10^9`

# Approach

Each exchange consumes `m` wrappers and returns 1 wrapper. Thus, every exchange decreases the total number of wrappers by `m - 1`.

To perform `x` exchanges, at least one wrapper must remain after the final exchange:

`n - x * (m - 1) >= 1`

Rearranging:

`x <= (n - 1) / (m - 1)`

Therefore, the maximum number of additional chocolates is:

`(n - 1) / (m - 1)`

Since integer division automatically rounds down for positive integers, the solution is straightforward.

# C++ Solution

```cpp
class Solution {
public:
    int chocolateExchange(int n, int m) {
        return (n - 1) / (m - 1);
    }
};
```

# Complexity Analysis

- **Time Complexity:** `O(1)`
- **Space Complexity:** `O(1)`
