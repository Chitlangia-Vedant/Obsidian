# [239. Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/)

You are given an array of integers `nums`, there is a sliding window of size `k` which is moving from the very left of the array to the very right. You can only see the `k` numbers in the window. Each time the sliding window moves right by one position.

Return _the max sliding window_.

**Example 1:**

**Input:** nums = [1,3,-1,-3,5,3,6,7], k = 3
**Output:** [3,3,5,5,6,7]
**Explanation:** 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       **3**
 1 [3  -1  -3] 5  3  6  7       **3**
 1  3 [-1  -3  5] 3  6  7       **5**
 1  3  -1 [-3  5  3] 6  7       **5**
 1  3  -1  -3 [5  3  6] 7       **6**
 1  3  -1  -3  5 [3  6  7]      **7**

**Example 2:**

**Input:** nums = [1], k = 1
**Output:** [1]

**Constraints:**

- `1 <= nums.length <= 105`
- `-104 <= nums[i] <= 104`
- `1 <= k <= nums.length`

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        
    }
};
```

# Solution

1. Initialize a map `m`, where the key is the element value and the value is its latest index in the current sliding window.
2. Create a result vector `res` of size `nums.size() - k + 1` to store the maximum element of each window.
3. Traverse the array `nums` from index `0` to `nums.size() - 1`.
4. Insert the current element into the map by storing its value as the key and its index as the value. If the value already exists, update its index to the current one.
5. If the current index `i` is greater than or equal to `k`, check whether the element leaving the sliding window (`nums[i-k]`) is stored in the map with index `i-k`.
6. If the above condition is true, erase that element from the map since its latest occurrence is no longer inside the current window.
7. Once the first window is formed (`i >= k-1`), the largest key in the map (`m.rbegin()->first`) represents the maximum element in the current sliding window.
8. Store this maximum value in `res[i-k+1]`.
9. Continue the process until all elements have been processed.
10. Return the result vector `res` containing the maximum element of every sliding window.
## CODE (Mine)

```cpp
class Solution {
public:

    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        map<int,int> m;
        vector<int> res(nums.size()-k+1);
        for(int i=0;i<nums.size();i++){
            m[nums[i]]=i;
            if(i>=k&&m[nums[i-k]]==(i-k)){
                m.erase(m.find(nums[i-k]));
            }
            if(i>=k-1)
                res[i-k+1]=m.rbegin()->first;
        }
        return res;
    }
};
```

# Solution 2

## Approach

```
Input: nums = [5,3,-1,-3,4], k = 3
```

In this case,

```csharp
[5,3,-1,-3,4], k = 3
 - - - → 5
   - -  - → 3
     -  -  - → 4

return [5,3,4]
```

If we sort all numbers in each window, we can get maximum number from the last index in the window. But sorting algorithm needs `O(nlogn)` which is a bit high cost.

**We can keep maximum number at the first index in each window, so that we can get maximum number easily.** 

To do that, we will use `deque`.

```sql
[5,3,-1,-3,4], k = 3

idx = 0 → current index
num = 5 → current number
q = [] → deque, keep maximum value at index 0
res = [] → return values
```

Let's see how it works!

First of all we have `5` at index `0`. Then check `deque` but no value, so just add `5` to `deque`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 0
num = 5
q = [5]
res = []
```

Next we are not sure if `5` is maximum number in the window, because length of window size doesn't reach `k` so far.

Let's move to next index.

```csharp
[5,3,-1,-3,4], k = 3

idx = 1
num = 3
q = [5]
res = []
```

Now we have `5` in `deque`. As I told you, we keep maximum number at index `0` in `deque`, so **compare current number with the last number in deque.  
**

```typescript
current number vs the latest number in deque
= 3 vs 5
```

We should keep `5` as a maximum value, so just add `3` to `deque`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 1
num = 3
q = [5,3]
res = []
```

Window size is still length of `2`, so skip adding maximum number to `res`.

Next,

```csharp
[5,3,-1,-3,4], k = 3

idx = 2
num = -1
q = [5,3]
res = []
```

Check current number vs the latest number in deque. We should keep `5` and `3` because they are greater than current number, so just add `-1` to `deque`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 2
num = -1
q = [5,3,-1]
res = []
```

Now window size is length of `3` which is equal to `k`. It's time to add maximum number to `res`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 2
num = -1
q = [5,3,-1]
res = [5]
```

Next

```csharp
[5,3,-1,-3,4], k = 3

idx = 3
num = -3
q = [5,3,-1]
res = [5]
```

Current number is less than the latest number in `deque`, so add `-3` to `deque`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 3
num = -3
q = [5,3,-1,-3]
res = [5]
```

Now `5` is maxinum number so far, but maxinum window size should be `k` which is `3` in this case. If we expand window size toward index `0`, window should be like this.

```csharp
[5,3,-1,-3,4], k = 3
   -  -  -
```

**Though `5` is maxinum number, it's out of bounds, so we should pop 5 from deque.**

```csharp
[5,3,-1,-3,4], k = 3

idx = 3
num = -3
q = [3,-1,-3]
res = [5]
```

Now `3` is maxinum number, so add `3` to `res`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 3
num = -3
q = [3,-1,-3]
res = [5,3]
```

Next

```csharp
[5,3,-1,-3,4], k = 3

idx = 4
num = 4
q = [3,-1,-3]
res = [5,3]
```

Check current number vs the latest number in `deque`. current number is greater than the latest number.

In this case, **there is possibility that current number is maximum number, so pop the latest number in deque.**

```csharp
[5,3,-1,-3,4], k = 3

idx = 4
num = 4
q = [3,-1]
res = [5,3]
```

We will repeat the same algorithm until we don't meet

```sql
current number > q[-1]

4 vs -1: pop -1
4 vs 3: pop 3

In the end, deque is empty and add current number to deque

[5,3,-1,-3,4], k = 3

idx = 4
num = 4
q = [4]
res = [5,3]
```

Now we check this window.

```csharp
[5,3,-1,-3,4], k = 3
      -  - - 
```

So just add `4` to `res`.

```csharp
[5,3,-1,-3,4], k = 3

idx = 4
num = 4
q = [4]
res = [5,3,4]
```

```kotlin
return [5,3,4]
```

---

⭐️ Points

1, Check current number is greater than the latest number in deque. If true, pop the latest number. Try to keep decreasing order.

2, If maximum number in deque is out of bounds, pop it.

3, If index is greater than or equal to `k - 1`, add maximum number to `res`.

- Why `k - 1`?

`k - 1` is maximum window size. The above example has `k = 3` which means maximum window size should be `3`.

We will use index number to check window size, so just convert real number to index number, **because real number starts from `1` but index number usually starts from index `0`.**

```typescript
1,2,3: real number
0,1,2: index number
```

## Complexity

- Time complexity: O(n)

- Space complexity: O(k)  
    Except `res`.

## CODE 

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        deque<int> deque;

        for (int idx = 0; idx < nums.size(); idx++) {
            int num = nums[idx];

            while (!deque.empty() && deque.back() < num) {
                deque.pop_back();
            }
            deque.push_back(num);

            if (idx >= k && nums[idx - k] == deque.front()) {
                deque.pop_front();
            }

            if (idx >= k - 1) {
                res.push_back(deque.front());
            }
        }

        return res;        
    }
};
```

## Step by Step algorithm

### Step 1: Initialize Result List and Deque

```python
res = []
q = deque()
```

- **Explanation**: We initialize an empty list `res` to store the results (the maximum values of each sliding window). We also initialize a deque `q` to store elements in a way that allows us to quickly access the maximum element of the current window.

### Step 2: Iterate Over the List

```python
for idx, num in enumerate(nums):
```

- **Explanation**: We iterate over the list `nums` using `enumerate` which gives us both the index `idx` and the current number `num`. This allows us to keep track of the position within the list.

### Step 3: Maintain Deque in Descending Order

```python
while q and q[-1] < num:
    q.pop()
q.append(num)
```

- **Explanation**: For each element in `nums`:
    - **Inner While Loop**: We remove elements from the back of the deque as long as the current element `num` is greater than the elements in the deque. This maintains the deque in a descending order of values.
    - **Append**: We append the current element `num` to the deque.

### Step 4: Remove an Element That Is Out of the Current Window

```python
if idx >= k and nums[idx - k] == q[0]:
    q.popleft()
```

- **Explanation**: If the index `idx` is greater than or equal to `k` (indicating that the window has moved forward):
    - We check if the element at the front of the deque is the same as the element that is now out of the window (i.e., `nums[idx - k]`).
    - If it is, we remove this element from the front of the deque (`popleft`).

### Step 5: Append Maximum of Current Window to Result

```python
if idx >= k - 1:
    res.append(q[0])
```

- **Explanation**: Once the index `idx` is greater than or equal to `k - 1` (indicating that we have processed at least one full window of size `k`):
    - We append the element at the front of the deque (`q[0]`) to the result list `res`. This element is the maximum value of the current window.

### Step 6: Return the Result List

```python
return res
```

- **Explanation**: After processing all elements in `nums`, we return the result list `res` which contains the maximum values for each sliding window.