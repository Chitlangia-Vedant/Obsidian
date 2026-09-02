[45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)

You are given a **0-indexed** array of integers `nums` of length `n`. You are initially positioned at index 0.

Each element `nums[i]` represents the maximum length of a forward jump from index `i`. In other words, if you are at index `i`, you can jump to any index `(i + j)` where:

- `0 <= j <= nums[i]` and
- `i + j < n`

Return _the minimum number of jumps to reach index_ `n - 1`. The test cases are generated such that you can reach index `n - 1`.

**Example 1:**

**Input:** nums = [2,3,1,1,4]
**Output:** 2
**Explanation:** The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.

**Example 2:**

**Input:** nums = [2,3,0,1,4]
**Output:** 2

**Constraints:**

- `1 <= nums.length <= 104`
- `0 <= nums[i] <= 1000`
- It's guaranteed that you can reach `nums[n - 1]`.
# Approach 1
## Intuition

At every index `n`, you try every possible jump:

```cpp
for(int i=1;i<=nums[n];i++)
```

For each possible next position `n+i`, recursively calculate the minimum jumps needed to reach the end.

So the main idea is:

```text
minimum jumps from n
= 1 + minimum jumps from any reachable next index
```

You store the answer for every index in `dp` so that the same index is never solved repeatedly.

## Algorithm

#### 1. Handle the trivial case

```cpp
if(nums.size()<=1) return 0;
```

If there is only one element, we are already at the last index.

---

#### 2. Initialize DP

```cpp
dp.assign(nums.size(),-1);
this->nums=nums;
```

`dp[i]` represents:

```text
minimum number of jumps needed to reach the last index from index i
```

`-1` means we haven't calculated it yet.

---

#### 3. Start recursion

```cpp
int res=solve(0);
```

We start at index `0`.

---

#### 4. Return memoized result

Inside `solve`:

```cpp
if(dp[n]!=-1) return dp[n];
```

If we have already calculated the answer for this index, return it immediately.

This prevents repeated work.

---

#### 5. Base case: already at the end

```cpp
if(n==nums.size()-1){
    dp[n]=0;
    return dp[n];
}
```

If we are already at the last index, we need `0` more jumps.

---

#### 6. Direct jump to the end

```cpp
if(n+nums[n]>=nums.size()-1){
    dp[n]=1;
    return dp[n];
}
```

If we can directly reach the last index from `n`, the answer is `1`.

For example:

```text
index = 1
nums[1] = 3
last index = 4

1 + 3 >= 4
```

So we can reach the end in one jump.

---

#### 7. Try every possible jump

```cpp
for(int i=1;i<=nums[n];i++){
```

From index `n`, we can jump to:

```text
n+1
n+2
...
n+nums[n]
```

For every possible destination:

```cpp
if(solve(n+i)==-1) continue;
```

If that position cannot reach the end, ignore it.

Otherwise:

```cpp
dp[n] = solve(n+i) + 1;
```

We add `1` for the current jump.

If multiple destinations are possible, take the minimum:

```cpp
dp[n]=min(dp[n],solve(n+i)+1);
```

## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> dp;
    vector<int> nums;
    int jump(vector<int>& nums) {
        if(nums.size()<=1) return 0;
        dp.assign(nums.size(),-1);
        this->nums=nums;
        int res=solve(0);
        for(int i:dp){
            cout<<i<<" ";
        }
        return res;
    }
    int solve(int n){
        if(dp[n]!=-1) return dp[n];
        if(n==nums.size()-1){
            dp[n]=0;
            return dp[n];
        }
        if(n+nums[n]>=nums.size()-1){
            dp[n]=1;
            return dp[n];
        }
        for(int i=1;i<=nums[n];i++){
            
            if(solve(n+i)==-1) continue;
            else if(dp[n]==-1) dp[n]=solve(n+i)+1;
            else dp[n]=min(dp[n],solve(n+i)+1);
        }
        return dp[n];
    }
};
```
## Dry Run

Let's use:

```text
nums = [2,3,1,1,4]
```

We start:

```text
solve(0)
```

From index `0`, we can jump to:

```text
1 or 2
```

So:

```text
solve(0)
├── solve(1)
└── solve(2)
```

#### `solve(1)`

```text
nums[1] = 3
```

We can jump to:

```text
2, 3, 4
```

Index `4` is the last index.

Therefore:

```text
solve(1) = 1
```

---

#### `solve(2)`

```text
nums[2] = 1
```

Only possible jump:

```text
2 → 3
```

Then:

```text
solve(3)
```

From index `3`:

```text
nums[3] = 1
```

So:

```text
3 → 4
```

Therefore:

```text
solve(3) = 1
solve(2) = 2
```

---

#### Back to `solve(0)`

We have:

```text
0 → 1 → 4
```

which requires:

```text
solve(1) + 1
= 1 + 1
= 2
```

The other path requires `3` jumps:

```text
0 → 2 → 3 → 4
```

So:

```text
solve(0) = 2
```

Final answer:

```text
2
```

## DP Structure

For this example, the important states are:

```text
dp[4] = 0
dp[3] = 1
dp[2] = 2
dp[1] = 1
dp[0] = 2
```

So the DP array represents:

```text
index:  0  1  2  3  4
dp:     2  1  2  1  0
```

## Why Memoization Is Needed

Without:

```cpp
if(dp[n]!=-1) return dp[n];
```

the same indices could be solved many times.

For example, both `solve(0)` and another recursive path may eventually call:

```text
solve(2)
```

With memoization, the first calculation stores:

```text
dp[2] = 2
```

and every later call simply returns `2`.

## Time & Space Complexity

For every index, we may try up to `nums[i]` possible jumps.

Therefore, in the worst case:

**Time:** `O(n²)`

**Space:** `O(n)`

`O(n)` comes from:
- `dp`
- recursion stack
- copied `nums` member

## Key Idea

Your solution asks the same question for every index:

```text
"What is the minimum number of jumps needed to reach the end from here?"
```

Then:

```text
solve(i) = 1 + min(solve(j))
```

for every reachable `j`.

The `dp` array remembers each answer so that each index is solved only once.

#### One important note

Your solution is correct, but **Jump Game II has an even better greedy solution** that runs in `O(n)` time and `O(1)` extra space.

The greedy idea is to treat all positions reachable with the current number of jumps as one "range" and, while scanning that range, find the position that can take us farthest next.

```text
Current range:  [0 ............... 2]
                    ↓
             farthest next range
                    [1 ........ 4]

Each time we finish the current range,
we must make another jump.
```

# Approach 2

```
Input: nums = [2,3,1,1,4]
```

We start from index `0`. In that case above, we have two choices. I mean jump to index `1` or index `2`.

---

Problem

We don't know whether we should jump to index `1` or index `2` to get minimum number of jumps in the end.

---

Let's think about index `1` and index `2` cases.

From index `1`, We have three choices. Jump to index `2`, `3` or `4`.  
From index `2`, We have one choice. Jump to index `3`.

---

⭐️　Points

Important thing is that farthest position(= index `2`) from previous jump position(= index `0`) is not always following the next farthest position.

From index `0`, the farthest position should be index `2`, because maximum jump from index `0` is `2`. But if we jump from index `2`, we can jump to the next position(= index `3`).

On the other hand, if we jump to index `1` from index `0`, we can jump to index `4` from index `1`. which is farther than index `3` from index `2`.

So my strategy is **to have near and far position and we check all jumps between the positions and get the farthest position every time.**

---

Let's see one by one.

```
Input: nums = [2,3,1,1,4]

near = 0
far = 0
jumps = 0
```

First of all, the range betwee `near` and `far` is `0`, so we check only index `0`. The farthest position should be

```java
farthest position = current index + maximum jump
= 0 + 2
= 2
```

We check all positions in the range.

Next, before we move to the next range, we should update `near`, `far` and `jumps`.

This question guarantee that we can definitely reach the last index, so at least, we must move forward from the current range, so

The next `near` position should be

```
far + 1
```

Because far position is the most right position of current range.

The next `far` position should be

```java
far = current farthest we found = 2
```

Of course, add +1 to jump times

```
jumps += 1
```

In the end,

```csharp
   n f
[2,3,1,1,4]

jumps = 1
```

Next we check between index `1` and index `2`.

From index `1`, the farthest position should

```
farthest = 1 + 3 = 4
```

From index `2`, the farthest position should

```
farthest = 2 + 1 = 3
```

We take index `4`. Then update `near`, `far` and `jumps`.

```
near = far + 1 = 3
far = farthest = 4
jumps = 1 + 1 = 2
```

In the end,

```csharp
       n f
[2,3,1,1,4]

jumps = 2
```

We will repeat the same algorithm. And now far position is reach the last index, so we stop iteration.

```kotlin
return 2(= jumps)
```

## Complexity

- Time complexity: O(n)

- Space complexity: O(1)
## CODE 

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int near = 0, far = 0, jumps = 0;

        while (far < nums.size() - 1) {
            int farthest = 0;
            for (int i = near; i <= far; i++) {
                farthest = max(farthest, i + nums[i]);
            }
            near = far + 1;
            far = farthest;
            jumps++;
        }

        return jumps;        
    }
};
```

## Step by Step Algorithm

1. **Initialization**:
    
    ```python
    near = far = jumps = 0
    ```
    
    - `near`: This variable represents the start of the current range of indices we are considering for jumps.
    - `far`: This variable represents the end of the current range of indices we are considering for jumps.
    - `jumps`: This variable keeps track of the number of jumps made.
2. **While Loop**:
    
    ```python
    while far < len(nums) - 1:
    ```
    
    - The loop continues until the `far` index reaches or exceeds the last index of the array (`len(nums) - 1`).
3. **Initialization of Farthest**:
    
    ```python
    farthest = 0
    ```
    
    - `farthest`: This variable will store the farthest index we can reach from the current range of indices (`near` to `far`).
4. **For Loop**:
    
    ```python
    for i in range(near, far + 1):
        farthest = max(farthest, i + nums[i])
    ```
    
    - This loop iterates through the current range of indices from `near` to `far`.
    - For each index `i`, it calculates `i + nums[i]` which is the farthest index we can reach by jumping from index `i`.
    - It updates `farthest` to be the maximum of its current value and `i + nums[i]`.
5. **Update Near and Far**:
    
    ```python
    near = far + 1
    far = farthest
    jumps += 1
    ```
    
    - `near`: Update the start of the next range to be one index after the current `far`.
    - `far`: Update the end of the next range to be `farthest` calculated in the for loop.
    - `jumps`: Increment the number of jumps made by 1.
6. **Return Statement**:
    
    ```python
    return jumps
    ```
    
    - After exiting the while loop, the function returns the total number of jumps made to reach the last index.