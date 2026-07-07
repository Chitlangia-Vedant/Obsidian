# [264. Ugly Number II](https://leetcode.com/problems/ugly-number-ii/)

An **ugly number** is a positive integer whose prime factors are limited to `2`, `3`, and `5`.

Given an integer `n`, return _the_ `nth` _**ugly number**_.

**Example 1:**

**Input:** n = 10
**Output:** 12
**Explanation:** [1, 2, 3, 4, 5, 6, 8, 9, 10, 12] is the sequence of the first 10 ugly numbers.

**Example 2:**

**Input:** n = 1
**Output:** 1
**Explanation:** 1 has no prime factors, therefore all of its prime factors are limited to 2, 3, and 5.

**Constraints:**

- `1 <= n <= 1690`

# Solution
We have an array _k_ of first n ugly number. We only know, at the beginning, the first one, which is 1. Then

k[1] = min( k[0]x2, k[0]x3, k[0]x5). The answer is k[0]x2. So we move 2's pointer to 1. Then we test:

k[2] = min( k[1]x2, k[0]x3, k[0]x5). And so on. Be careful about the cases such as 6, in which we need to forward both pointers of 2 and 3.

x here is multiplication.

# CODE

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        if(n <= 0) return false; // get rid of corner cases 
        if(n == 1) return true; // base case
        int t2 = 0, t3 = 0, t5 = 0; //pointers for 2, 3, 5
        vector<int> k(n);
        k[0] = 1;
        for(int i  = 1; i < n ; i ++)
        {
            k[i] = min(k[t2]*2,min(k[t3]*3,k[t5]*5));
            if(k[i] == k[t2]*2) t2++; 
            if(k[i] == k[t3]*3) t3++;
            if(k[i] == k[t5]*5) t5++;
        }
        return k[n-1];
    }
};
```

# Dry Run of `nthUglyNumber(n)` for `n = 10`

Ugly numbers are numbers whose prime factors are only `2`, `3`, and `5`.

Initial state:

```cpp
n = 10

t2 = 0
t3 = 0
t5 = 0

k = [1, _, _, _, _, _, _, _, _, _]
```

`k[0] = 1` because the 1st ugly number is `1`.

---

### Iteration 1 (`i = 1`)

Candidates:

```cpp
k[t2] * 2 = k[0] * 2 = 2
k[t3] * 3 = k[0] * 3 = 3
k[t5] * 5 = k[0] * 5 = 5
```

Minimum:

```cpp
k[1] = 2
```

Update pointers:

```cpp
2 == k[t2] * 2  → t2++

t2 = 1
t3 = 0
t5 = 0
```

Array:

```cpp
k = [1, 2, _, _, _, _, _, _, _, _]
```

---

### Iteration 2 (`i = 2`)

Candidates:

```cpp
k[1] * 2 = 4
k[0] * 3 = 3
k[0] * 5 = 5
```

Minimum:

```cpp
k[2] = 3
```

Update pointers:

```cpp
3 == k[t3] * 3 → t3++

t2 = 1
t3 = 1
t5 = 0
```

Array:

```cpp
k = [1, 2, 3, _, _, _, _, _, _, _]
```

---

### Iteration 3 (`i = 3`)

Candidates:

```cpp
k[1] * 2 = 4
k[1] * 3 = 6
k[0] * 5 = 5
```

Minimum:

```cpp
k[3] = 4
```

Update pointers:

```cpp
4 == k[t2] * 2 → t2++

t2 = 2
t3 = 1
t5 = 0
```

Array:

```cpp
k = [1, 2, 3, 4, _, _, _, _, _, _]
```

---

### Iteration 4 (`i = 4`)

Candidates:

```cpp
k[2] * 2 = 6
k[1] * 3 = 6
k[0] * 5 = 5
```

Minimum:

```cpp
k[4] = 5
```

Update pointers:

```cpp
5 == k[t5] * 5 → t5++

t2 = 2
t3 = 1
t5 = 1
```

Array:

```cpp
k = [1, 2, 3, 4, 5, _, _, _, _, _]
```

---

### Iteration 5 (`i = 5`)

Candidates:

```cpp
k[2] * 2 = 6
k[1] * 3 = 6
k[1] * 5 = 10
```

Minimum:

```cpp
k[5] = 6
```

Update pointers:

```cpp
6 == k[t2] * 2 → t2++
6 == k[t3] * 3 → t3++

t2 = 3
t3 = 2
t5 = 1
```

Notice both pointers move to avoid duplicates.

Array:

```cpp
k = [1, 2, 3, 4, 5, 6, _, _, _, _]
```

---

### Iteration 6 (`i = 6`)

Candidates:

```cpp
k[3] * 2 = 8
k[2] * 3 = 9
k[1] * 5 = 10
```

Minimum:

```cpp
k[6] = 8
```

Update pointers:

```cpp
8 == k[t2] * 2 → t2++

t2 = 4
t3 = 2
t5 = 1
```

Array:

```cpp
k = [1, 2, 3, 4, 5, 6, 8, _, _, _]
```

---

### Iteration 7 (`i = 7`)

Candidates:

```cpp
k[4] * 2 = 10
k[2] * 3 = 9
k[1] * 5 = 10
```

Minimum:

```cpp
k[7] = 9
```

Update pointers:

```cpp
9 == k[t3] * 3 → t3++

t2 = 4
t3 = 3
t5 = 1
```

Array:

```cpp
k = [1, 2, 3, 4, 5, 6, 8, 9, _, _]
```

---

### Iteration 8 (`i = 8`)

Candidates:

```cpp
k[4] * 2 = 10
k[3] * 3 = 12
k[1] * 5 = 10
```

Minimum:

```cpp
k[8] = 10
```

Update pointers:

```cpp
10 == k[t2] * 2 → t2++
10 == k[t5] * 5 → t5++

t2 = 5
t3 = 3
t5 = 2
```

Again, multiple pointers move to prevent duplicate 10.

Array:

```cpp
k = [1, 2, 3, 4, 5, 6, 8, 9, 10, _]
```

---

### Iteration 9 (`i = 9`)

Candidates:

```cpp
k[5] * 2 = 12
k[3] * 3 = 12
k[2] * 5 = 15
```

Minimum:

```cpp
k[9] = 12
```

Update pointers:

```cpp
12 == k[t2] * 2 → t2++
12 == k[t3] * 3 → t3++

t2 = 6
t3 = 4
t5 = 2
```

Array:

```cpp
k = [1, 2, 3, 4, 5, 6, 8, 9, 10, 12]
```

---

### Final Result

The first 10 ugly numbers are:

```cpp
1, 2, 3, 4, 5, 6, 8, 9, 10, 12
```

Therefore,

```cpp
return k[n-1];
return k[9];
return 12;
```

---

### Pointer Intuition

- `t2` points to the smallest ugly number whose multiple by `2` has not been used.
- `t3` points to the smallest ugly number whose multiple by `3` has not been used.
- `t5` points to the smallest ugly number whose multiple by `5` has not been used.
- If multiple candidates produce the same ugly number (like `6 = 2×3 = 3×2`), all corresponding pointers are incremented to avoid duplicates.