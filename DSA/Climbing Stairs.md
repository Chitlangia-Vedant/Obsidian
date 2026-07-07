# [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example 1:**

**Input:** n = 2
**Output:** 2
**Explanation:** There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**

**Input:** n = 3
**Output:** 3
**Explanation:** There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

**Constraints:**

- `1 <= n <= 45`

```Java
class Solution {
    public int climbStairs(int n) {
        
    }
}
```

# Understanding:

Order Does Matter:
[1,2] and [2,1] ------> Different Solutions: Count = 2

Order Does Not Matter:
[1,2] and [2,1] ------> Same Solutions: Count = 1

```
Example Input:
N = 4

Ways:

[1 1 1 1]
[1 2 1]
[2 1 1]
[1 1 2]
[2 2]

OP: 5
```

# Solution:

## (1) Identify:
"Count the number of ways"

## (2) Decide a State Expression

state(N) = Number of Ways to reach Nth Stair using 1 or 2 Steps
state(K) = Number of Ways to reach Kth Stair using 1 or 2 Steps

TRICK:

NOTE = state(N) - ALWAYS MENTIONED IN THE QUESTION
state(k) = Replace N with K

## (3) Formulate a State Relation ----- IMP

- How does the current state result relates to previous results

state(k) -----> k-1, k-2 etc

ANS: state(k) = state(k-1) + state(k-2)

Reach: 5th Stair

4th Stair: 1 Step
3rd Stair: 2 Steps

Number of Ways to Reach 5th Stair = 
Number of Ways to Reach 4th Stair
 \+ (OR)
Number of Ways to Reach 3rd Stair

```
state(5) = state(4) + state(3)
		 = state(5-1) + state(5-2)

state(k) = state(k-1) + state(k-2)
```

## Fibonacci Sequence

```
IP: 1
OP: 1

IP: 2
OP: 2

IP: 3
OP: 3

IP: 4
OP: 5

OP:

1 2 3 5 8 13 21...... -----> Fibonacci
```

# CODE
```cpp
class Solution
{
    public:
    int climbStairs(int n)
    {
        
        if (n == 0 || n == 1 || n == 2)
            return n;
        
        int stairs[n];
        stairs[0]=1;
        stairs[1] =2;
            
        for (int i=2; i<n; i++)
        {
            stairs[i] = stairs[i-1] + stairs[i-2];
        }
        
        return stairs[n-1];
    }
};
```

TC: O(N)
SC: O(N)

# Same Question
- Fibonacci
- Max Steps: Bottom to Top (0 to N)
- Max Steps: Top to Bottom (N to 0)
- Find Number of Ways to make N as sum of {1,2}
- Find Number of Ways to make N to 0 by Removing {1,2}