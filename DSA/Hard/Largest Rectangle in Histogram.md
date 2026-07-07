# [84. Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/)

Given an array of integers `heights` representing the histogram's bar height where the width of each bar is `1`, return _the area of the largest rectangle in the histogram_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram.jpg)

**Input:** heights = [2,1,5,6,2,3]
**Output:** 10
**Explanation:** The above is a histogram where width of each bar is 1.
The largest rectangle is shown in the red area, which has an area = 10 units.

**Example 2:**

![](https://assets.leetcode.com/uploads/2021/01/04/histogram-1.jpg)

**Input:** heights = [2,4]
**Output:** 4

**Constraints:**

- `1 <= heights.length <= 105`
- `0 <= heights[i] <= 104`

# Solution
## 1. Brute Force

### Intuition

For every bar, we try to treat it as the **shortest bar** in the rectangle.  
To find how wide this rectangle can extend, we look **left** and **right** until we hit a bar shorter than the current one.  
The width between these two boundaries gives the largest rectangle where this bar is the limiting height.  
We repeat this for every bar and keep track of the maximum rectangle found.

### Algorithm

1. Let `maxArea` store the largest rectangle found.
2. For each index `i`:
    - Let `height` be the height of the current bar.
    - Expand to the **right** until you find a bar shorter than `height`.
    - Expand to the **left** while bars are not shorter than `height`.
    - Compute the width between the boundaries.
    - Update `maxArea` with `height * width`.
3. Return `maxArea`.

## CODE
Largest Rectangle In Histogram - Brute Force

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        int maxArea = 0;

        for (int i = 0; i < n; i++) {
            int height = heights[i];

            int rightMost = i + 1;
            while (rightMost < n && heights[rightMost] >= height) {
                rightMost++;
            }

            int leftMost = i;
            while (leftMost >= 0 && heights[leftMost] >= height) {
                leftMost--;
            }

            rightMost--;
            leftMost++;
            maxArea = max(maxArea, height * (rightMost - leftMost + 1));
        }
        return maxArea;
    }
};
```

### Time & Space Complexity

- Time complexity: O(n2)O(n2)
- Space complexity: O(1)O(1)

---

## 2. Divide And Conquer (Segment Tree)

### Intuition

A large rectangle in the histogram must have some bar as its **shortest bar**.  
If we know the index of the **minimum height bar** in any range, then:

- The largest rectangle that **uses that bar as the limiting height** spans the entire range.
- Anything larger must lie **entirely on the left** or **entirely on the right** of that bar.

So we can solve the problem with a **divide and conquer** idea:

1. In a given range `[L, R]`, find the index of the smallest bar.
2. Compute:
    - The area using this bar across the whole range.
    - The best area entirely in `[L, minIndex - 1]`.
    - The best area entirely in `[minIndex + 1, R]`.
3. The answer for `[L, R]` is the maximum of those three.

To find the **index of the minimum height quickly** for any range, we use a **segment tree** built on heights. It supports “min index in range” queries in `O(log n)` time, which makes the whole divide-and-conquer efficient.

### Algorithm

1. Build a **segment tree** over the heights array:
    - Each node stores the index of the minimum height in its segment.
2. Define a recursive function `solve(L, R)`:
    - If `L > R`, return `0`.
    - If `L == R`, return `heights[L]` (only one bar).
    - Use the segment tree to find `minIndex` = index of the smallest bar in `[L, R]`.
    - Compute:
        - `area_with_min = heights[minIndex] * (R - L + 1)`
        - `area_left = solve(L, minIndex - 1)`
        - `area_right = solve(minIndex + 1, R)`
    - Return `max(area_with_min, area_left, area_right)`.
3. The final answer is `solve(0, n - 1)` where `n` is the number of bars.
## CODE
Largest Rectangle In Histogram - Segment Tree

```cpp
class MinIdx_Segtree {
public:
    int n;
    const int INF = 1e9;
    vector<int> A;
    vector<int> tree;
    MinIdx_Segtree(int N, vector<int>& a) {
        this->n = N;
        this->A = a;
        while (__builtin_popcount(n) != 1) {
            A.push_back(INF);
            n++;
        }
        tree.resize(2 * n);
        build();
    }

    void build() {
        for (int i = 0; i < n; i++) {
            tree[n + i] = i;
        }
        for (int j = n - 1; j >= 1; j--) {
            int a = tree[j<<1];
            int b = tree[(j<<1) + 1];
            if(A[a]<=A[b])tree[j]=a;
            else tree[j] = b;
        }
    }

    void update(int i, int val) {
        A[i] = val;
        for (int j = (n + i) >> 1; j >= 1; j >>= 1) {
            int a = tree[j<<1];
            int b = tree[(j<<1) + 1];
            if(A[a]<=A[b])tree[j]=a;
            else tree[j] = b;
        }
    }

    int query(int ql, int qh) {
        return query(1, 0, n - 1, ql, qh);
    }

    int query(int node, int l, int h, int ql, int qh) {
        if (ql > h || qh < l) return INF;
        if (l >= ql && h <= qh) return tree[node];
        int a = query(node << 1, l, (l + h) >> 1, ql, qh);
        int b = query((node << 1) + 1, ((l + h) >> 1) + 1, h, ql, qh);
        if(a==INF)return b;
        if(b==INF)return a;
        return A[a]<=A[b]?a:b;
    }
};

class Solution {
public:
    int getMaxArea(vector<int>& heights, int l, int r, MinIdx_Segtree& st) {
        if (l > r) return 0;
        if (l == r) return heights[l];

        int minIdx = st.query(l, r);
        return max(max(getMaxArea(heights, l, minIdx - 1, st),
                   getMaxArea(heights, minIdx + 1, r, st)),
                   (r - l + 1) * heights[minIdx]);
    }
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        MinIdx_Segtree st(n, heights);
        return getMaxArea(heights, 0, n - 1, st);
    }
};
```

### Time & Space Complexity

- Time complexity: O(nlog⁡n)O(nlogn)
- Space complexity: O(n)O(n)

---

## 3. Stack

### Intuition

For each bar, we want to know how far it can stretch left and right **before bumping into a shorter bar**.  
That distance tells us the widest rectangle where this bar is the limiting height.  
To efficiently find the nearest smaller bar on both sides, we use a **monotonic stack** that keeps indices of bars in increasing height order.  
This lets us compute boundaries in linear time instead of checking outward for every bar.

### Algorithm

1. Use a stack to find, for each index `i`, the nearest smaller bar on the **left**:
    - If the current bar is shorter than the bar on top of the stack, pop until this is no longer true.
    - The top of the stack becomes the left boundary.
    - If the stack is empty, no smaller bar exists, so left boundary is `-1`.
2. Repeat the same process from right to left to find the nearest smaller bar on the **right**:
    - If no smaller bar exists, the right boundary is `n`.
3. For each bar:
    - Expand between the boundaries and compute its max rectangle:  
        `height[i] * (right - left - 1)`
4. Return the largest area found.
## CODE
Largest Rectangle In Histogram - Monotonic Stack

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        vector<int> leftMost(n, -1);
        vector<int> rightMost(n, n);
        stack<int> stack;

        for (int i = 0; i < n; i++) {
            while (!stack.empty() && heights[stack.top()] >= heights[i]) {
                stack.pop();
            }
            if (!stack.empty()) {
                leftMost[i] = stack.top();
            }
            stack.push(i);
        }

        while (!stack.empty()) stack.pop();

        for (int i = n - 1; i >= 0; i--) {
            while (!stack.empty() && heights[stack.top()] >= heights[i]) {
                stack.pop();
            }
            if (!stack.empty()) {
                rightMost[i] = stack.top();
            }
            stack.push(i);
        }

        int maxArea = 0;
        for (int i = 0; i < n; i++) {
            leftMost[i] += 1;
            rightMost[i] -= 1;
            maxArea = max(maxArea, heights[i] * (rightMost[i] - leftMost[i] + 1));
        }

        return maxArea;
    }
};
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---

## 4. Stack (One Pass)

### Intuition

We want, for each bar, the widest area where it can act as the **shortest bar**.  
With a single pass and a stack, we can do this on the fly:

- We keep a stack of bars in **increasing height order**, each stored with the earliest index where that height can start.
- When we see a new bar that is **shorter** than the top of the stack, it means the taller bar on top can’t extend further to the right.
    - So we pop it and compute the area it could cover.
- The new shorter bar can start from as far left as the popped bar’s start index, so we reuse that index.
- After the pass, we compute areas for any bars still in the stack, extending them to the end.

Each bar is pushed and popped at most once, giving an efficient, one-pass solution.

### Algorithm

1. Initialize:
    - An empty stack to store pairs `(start_index, height)`.
    - `maxArea = 0`.
2. Traverse the histogram from left to right with index `i` and height `h`:
    - Set `start = i`.
    - While the stack is not empty and the top height is **greater than** `h`:
        - Pop `(index, height)` from the stack.
        - Update `maxArea` with `height * (i - index)`.
        - Set `start = index` (the new bar can start from here).
    - Push `(start, h)` onto the stack.
3. After the loop, process remaining bars in the stack:
    - For each `(index, height)` in the stack:
        - Update `maxArea` with `height * (n - index)`, where `n` is the total number of bars.
4. Return `maxArea`.
## CODE
Largest Rectangle In Histogram - Monotonic Stack

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int maxArea = 0;
        stack<pair<int, int>> stack; // pair: (index, height)

        for (int i = 0; i < heights.size(); i++) {
            int start = i;
            while (!stack.empty() && stack.top().second > heights[i]) {
                pair<int, int> top = stack.top();
                int index = top.first;
                int height = top.second;
                maxArea = max(maxArea, height * (i - index));
                start = index;
                stack.pop();
            }
            stack.push({ start, heights[i] });
        }

        while (!stack.empty()) {
            int index = stack.top().first;
            int height = stack.top().second;
            maxArea = max(maxArea, height * (static_cast<int>(heights.size()) - index));
            stack.pop();
        }
        return maxArea;
    }
};
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)

---

## 5. Stack (Optimal)

### Intuition

We want, for every bar, to know how wide it can stretch while still being the **shortest bar** in that rectangle.

A monotonic stack helps with this:

- We keep a stack of indices where the bar heights are in **increasing order**.
- As long as the next bar is **taller or equal**, we keep pushing indices.
- When we see a **shorter** bar, it means the bar on top of the stack can’t extend further to the right:
    - We pop it and treat it as the height of a rectangle.
    - Its width goes from the new top of the stack + 1 up to the current index − 1.

To make sure every bar eventually gets popped and processed, we run the loop one extra step with a “virtual” bar of height 0 at the end.  
Each bar is pushed and popped at most once, so this is both optimal and clean.

### Algorithm

1. Initialize:
    - `maxArea = 0`
    - An empty stack to store indices of bars (with heights in increasing order).
2. Loop `i` from `0` to `n` (inclusive):
    - While the stack is not empty **and** either:
        - `i == n` (we're past the last bar, acting like height `0`), or
        - `heights[i]` is **less than or equal** to the height at the top index of the stack:
            - Pop the top index; let its height be `h`.
            - Compute the width:
                - If the stack is empty, width = `i` (it extends from `0` to `i - 1`).
                - Otherwise, width = `i - stack.top() - 1`.
            - Update `maxArea` with `h * width`.
    - Push the current index `i` onto the stack.
3. After the loop, `maxArea` holds the largest rectangle area. Return it.
## CODE
Largest Rectangle In Histogram - Monotonic Stack

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int n = heights.size();
        int maxArea = 0;
        stack<int> stack;

        for (int i = 0; i <= n; i++) {
            while (!stack.empty() &&
                 (i == n || heights[stack.top()] >= heights[i])) {
                int height = heights[stack.top()];
                stack.pop();
                int width = stack.empty() ? i : i - stack.top() - 1;
                maxArea = max(maxArea, height * width);
            }
            stack.push(i);
        }
        return maxArea;
    }
};
```

### Time & Space Complexity

- Time complexity: O(n)O(n)
- Space complexity: O(n)O(n)