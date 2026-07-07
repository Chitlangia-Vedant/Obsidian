# [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/)

The **Fibonacci numbers**, commonly denoted `F(n)` form a sequence, called the **Fibonacci sequence**, such that each number is the sum of the two preceding ones, starting from `0` and `1`. That is,

F(0) = 0, F(1) = 1
F(n) = F(n - 1) + F(n - 2), for n > 1.

Given `n`, calculate `F(n)`.

**Example 1:**

**Input:** n = 2
**Output:** 1
**Explanation:** F(2) = F(1) + F(0) = 1 + 0 = 1.

**Example 2:**

**Input:** n = 3
**Output:** 2
**Explanation:** F(3) = F(2) + F(1) = 1 + 1 = 2.

**Example 3:**

**Input:** n = 4
**Output:** 3
**Explanation:** F(4) = F(3) + F(2) = 2 + 1 = 3.

**Constraints:**

- `0 <= n <= 30`

```cpp
class Solution {
public:
    int fib(int n) {
        
    }
};
```

# Solution:

## (1) Recursion:
TC: O(2^N)
SC: O(N) - Recursion Space/Stack
## (2) Iteration:
Take first and second

first + second

TC: O(N)
SC: O(1)

## (3) Use DP:
Maintain States and return fib[n-1]

TC: O(N)
SC: O(N) - Array Storing the results

## (4) Binets Formula: Maths Based Solution

Phi = (sqrt(5) + 1)/2

fib(n) = (phi^n)/sqrt(5)

TC: O(1)
SC: O(1)
# CODE:

```cpp
class Solution {
public:
    int fib(int n) 
    {
        double phi = (sqrt(5) + 1)/2;
        return round(pow(phi,n)/sqrt(5));
    }
};
```

TC: O(1)
SC: O(1)
