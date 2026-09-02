[69. Sqrt(x)](https://leetcode.com/problems/sqrtx/)

Given a non-negative integer `x`, return _the square root of_ `x` _rounded down to the nearest integer_. The returned integer should be **non-negative** as well.

You **must not use** any built-in exponent function or operator.

- For example, do not use `pow(x, 0.5)` in c++ or `x ** 0.5` in python.

**Example 1:**

**Input:** x = 4
**Output:** 2
**Explanation:** The square root of 4 is 2, so we return 2.

**Example 2:**

**Input:** x = 8
**Output:** 2
**Explanation:** The square root of 8 is 2.82842..., and since we round it down to the nearest integer, 2 is returned.

**Constraints:**

- `0 <= x <= 231 - 1`
# Approach - Brute force solution

```python
class Solution(object):
    def mySqrt(self, x):
        if x < 2:
            return x
        
        i = 2
        while i * i <= x:
            i += 1
        
        return i - 1
        
```

---

## Solution1: Understanding the Core of the Problem

The original problem can be stated as:

“Compute the square root of a non-negative integer x, and return only the integer part (rounded down).”

Examples:

```sql
x = 8 → √8 ≈ 2.828 → Result = 2
x = 9 → √9 = 3 → Result = 3
```

We can reframe this problem as:

```sql
“Find the largest integer m such that m^2 <= x.”
```

This new formulation is crucial—it sets the stage for a **binary search** solution.

---

## Why Not Just Use a Linear Search?

##### The Search Space is Finite

We know the square root of x must lie between 0 and x.  
In fact, for x >= 2, we can safely limit our search to the range 1 to x // 2, because (x // 2)^2 will already exceed x.

So we have a finite, well-bounded range to search.

##### The Key Insight: Monotonicity

Here’s the most important observation:

---

⭐️ Points

As m increases, m^2 also increases(monotonicity).  
If m^2 < x, we need to try a larger m.  
If m^2 > x, we try a smaller m.