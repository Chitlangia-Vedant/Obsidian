[77. Combinations](https://leetcode.com/problems/combinations/)

Given two integers `n` and `k`, return _all possible combinations of_ `k` _numbers chosen from the range_ `[1, n]`.

You may return the answer in **any order**.

**Example 1:**

**Input:** n = 4, k = 2
**Output:** [[1,2],[1,3],[1,4],[2,3],[2,4],[3,4]]
**Explanation:** There are 4 choose 2 = 6 total combinations.
Note that combinations are unordered, i.e., [1,2] and [2,1] are considered to be the same combination.

**Example 2:**

**Input:** n = 1, k = 1
**Output:** [[1]]
**Explanation:** There is 1 choose 1 = 1 total combination.

**Constraints:**

- `1 <= n <= 20`
- `1 <= k <= n`
# Intuition

For every number `n`, there are only two possibilities:

1. Don't include `n` → find all `k`-combinations from `[1, n-1]`.
2. Include `n` → find all `(k-1)`-combinations from `[1, n-1]`, then add `n` to each one.

So:

```text
C(n,k) = C(n-1,k) + C(n-1,k-1)
```

Your recursion directly follows this idea.

The first part:

```cpp
auto a = combine(n-1, k);
```

gives all combinations that **don't contain `n`**.

The second part:

```cpp
auto b = combine(n-1, k-1);
```

gives all combinations where we still need to choose `k-1` numbers, because `n` will be the final chosen number.

Then:

```cpp
for(auto& x:b){
    x.push_back(n);
}
```

adds `n` to every combination in `b`.

Finally, combine `a` and `b`.

# Algorithm

### 1. Base case: `k == 0`

```cpp
if(k==0) return {{}};
```

There is exactly one way to choose 0 numbers:

```text
{}
```

This is important because it allows the recursion to build combinations correctly.

For example:

```text
combine(3,1)
    → combine(2,0)
    → {{}}
    → add 3
    → {3}
```

---

### 2. Base case: `n == k`

```cpp
if(n==k){
    vector<int> res;
    for(int i=1;i<=n;i++){
        res.push_back(i);
    }
    return {res};
}
```

If we need to choose exactly `n` numbers from `[1,n]`, there is only one possible combination:

```text
[1,2,...,n]
```

For example:

```text
combine(4,4)
→ [[1,2,3,4]]
```

---

### 3. Don't include `n`

```cpp
auto a = combine(n-1, k);
```

We ignore `n` and choose `k` numbers from:

```text
[1, n-1]
```

---

### 4. Include `n`

```cpp
auto b = combine(n-1, k-1);
```

If `n` is included, we only need `k-1` more numbers.

So we choose:

```text
k-1 numbers from [1,n-1]
```

Then append `n`:

```cpp
for(auto& x:b){
    x.push_back(n);
}
```

---

### 5. Merge both results

```cpp
a.insert(a.end(),
    make_move_iterator(b.begin()),
    make_move_iterator(b.end()));
```

`a` contains combinations without `n`.

`b` contains combinations with `n`.

Move all of `b` into `a` and return it.

# CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        if(k==0) return {{}};
        if(n==k){
            vector<int> res;
            for(int i=1;i<=n;i++){
                res.push_back(i);
            }
            return {res};
        } 
        auto a = combine(n-1, k);
        auto b = combine(n-1, k-1);
        for(auto& x:b){
            x.push_back(n);
        }
        a.insert(a.end(),
         make_move_iterator(b.begin()),
         make_move_iterator(b.end()));
        return a;
    }
};
```
# Dry Run

Let's take:

```text
n = 4
k = 2
```

We want:

```text
C(4,2)
```

The recursion splits:

```text
C(4,2)
├── C(3,2)       ← don't take 4
└── C(3,1) + 4   ← take 4
```

### `C(3,2)`

```text
C(3,2)
├── C(2,2)
└── C(2,1) + 3
```

`C(2,2)`:

```text
[[1,2]]
```

`C(2,1)`:

```text
[[1],[2]]
```

Add `3`:

```text
[[1,3],[2,3]]
```

Therefore:

```text
C(3,2)
= [[1,2],[1,3],[2,3]]
```

---

### `C(3,1) + 4`

`C(3,1)` gives:

```text
[[1],[2],[3]]
```

Add `4` to each:

```text
[[1,4],[2,4],[3,4]]
```

---

### Combine both

```text
[[1,2],
 [1,3],
 [2,3],
 [1,4],
 [2,4],
 [3,4]]
```

Which gives all:

```text
4 choose 2 = 6
```

# Why This Works

Every combination of `k` numbers from `[1,n]` either:

- **contains `n`**, or
- **doesn't contain `n`**.

These two cases are mutually exclusive and cover every possible combination.

Therefore:

```text
C(n,k) = C(n-1,k) + C(n-1,k-1)
```

Your code is essentially implementing this recurrence directly.

# Time & Space Complexity

There are `C(n,k)` combinations in the answer.

Each combination contains `k` elements, so generating the output itself requires:

**Time:** `O(C(n,k) × k)`

**Space:** `O(C(n,k) × k)`

The recursion itself uses `O(n)` stack space, but the dominant space is the returned combinations.

# Key Idea

Think of every number as a **take / don't-take decision**:

```text
Don't take n → combine(n-1, k)
Take n      → combine(n-1, k-1) + n
```

So the whole solution comes directly from:

```text
C(n,k) = C(n-1,k) + C(n-1,k-1)
```
