[287. Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/)

Given an array of integers `nums` containing `n + 1` integers where each integer is in the range `[1, n]` inclusive.

There is only **one repeated number** in `nums`, return _this repeated number_.

You must solve the problem **without** modifying the array `nums` and using only constant extra space.

**Example 1:**

**Input:** nums = [1,3,4,2,2]
**Output:** 2

**Example 2:**

**Input:** nums = [3,1,3,4,2]
**Output:** 3

**Example 3:**

**Input:** nums = [3,3,3,3,3]
**Output:** 3

**Constraints:**

- `1 <= n <= 105`
- `nums.length == n + 1`
- `1 <= nums[i] <= n`
- All the integers in `nums` appear only **once** except for **precisely one integer** which appears **two or more** times.

**Follow up:**

- How can we prove that at least one duplicate number must exist in `nums`?
- Can you solve the problem in linear runtime complexity?
# Brute Force (2 Loops)

Since the problem requires solving without modifying the array `nums` and using only constant extra space, we can use Brute Force. It's easy to use **2 loops** to do it, but the time complexity is `O(n²)`, so it would time out.

```cpp
// 2 Loops
public:
int findDuplicate_2loops(vector<int>& nums) {
    int len = nums.size();
    for (int i = 0; i < len; i++) {
        for (int j = i + 1; j < len; j++) {
            if (nums[i] == nums[j]) {
                return nums[i];
            }
        }
    }

    return len;
}
```

## Analysis

- **Time Complexity**: `O(n²)`
- **Space Complexity**: `O(1)`

# Count

Count the frequency of the numbers in the array. With extra `O(n)` space, without modifying the input.

```cpp
int findDuplicate(vector<int>& nums) {
    int len = nums.size();
    vector<int> cnt(len + 1, 0);

    for (int i = 0; i < len; i++) {
        cnt[nums[i]]++;
        if (cnt[nums[i]] > 1) {
            return nums[i];
        }
    }

    return len;
}
```

## Analysis

- **Time Complexity**: `O(n)`
- **Space Complexity**: `O(n)`

# Hash

Using an `unordered_set` to record the occurrence of each number. With extra `O(n)` space, without modifying the input.

```cpp
int findDuplicate_set(vector<int>& nums) {
    unordered_set<int> set;
    int len = nums.size();

    for (int i = 0; i < len; i++) {
        if (set.find(nums[i]) != set.end()) {
            return nums[i];
        }
        set.insert(nums[i]);
    }

    return len;
}
```

## Analysis

- **Time Complexity**: `O(n)`
- **Space Complexity**: `O(n)`

# Marking visited value within the array

Since all values of the array are between `[1...n]` and the array size is `n+1`, while scanning the array from left to right, we set `nums[n]` to its negative value. With extra `O(1)` space, modifying the input.

```cpp
// Visited
int findDuplicate_mark(vector<int>& nums) {
    int len = nums.size();

    for (int num : nums) {
        int idx = abs(num);

        if (nums[idx] < 0) {
            return idx;
        }

        nums[idx] = -nums[idx];
    }

    return len;
}
```

## Analysis

- **Time Complexity**: `O(n)`
- **Space Complexity**: `O(1)`

# Sort

Sort the array first, then use a loop from `1` to `n`. With `O(nlogn)` time and modifying the input.

```cpp
int findDuplicate_sort(vector<int>& nums) {
    sort(nums.begin(), nums.end());

    int len = nums.size();

    for (int i = 1; i < len; i++) {
        if (nums[i] == nums[i - 1]) {
            return nums[i];
        }
    }

    return len;
}
```

## Analysis

- **Time Complexity**: `O(nlogn)`
- **Space Complexity**: `O(logn)`

# Index Sort

If the array is sorted, the value of each array element is its index value `index+1`, then we can do this:

1. If `nums[i]==i+1`, it means that the order has been sorted, then skip, `i++`;
2. If `nums[i]==nums[nums[i]-1]`, it means that there is already a value at the correct index, then this value is a duplicated element;
3. If none of the above is satisfied, exchange the values of `nums[i]` and `nums[nums[i]-1]`.

With extra `O(1)` space, modifying the input.

```cpp
// Index Sort
// n + 1 numbers in n.
int findDuplicate_index_sort(vector<int>& nums) {
    int len = nums.size();

    for (int i = 0; i < len; ) {
        int n = nums[i];

        if (n == i + 1) {
            i++;
        }
        else if (n == nums[n - 1]) {
            return n;
        }
        else {
            nums[i] = nums[n - 1];
            nums[n - 1] = n;
        }
    }

    return 0;
}
```

## Analysis

- **Time Complexity**: `O(n)`
- **Space Complexity**: `O(1)`

# Binary Search

The key is to find an integer in the array `[1,2,...,n]` instead of finding an integer in the **input array**.

We can use binary search. Each round we guess one number, scan the input array, narrow the search range, and finally get the answer.

According to the **Pigeonhole Principle**, `n+1` integers placed in an array of length `n` must contain at least one repeated integer.

For a guessed number `mid`, count how many elements are less than or equal to `mid`.

1. If `cnt` is strictly greater than `mid`, the duplicate is in `[left,mid]`.
2. Otherwise, the duplicate is in `[mid+1,right]`.

With extra `O(1)` space, without modifying the input.

```cpp
int findDuplicate_bs(vector<int>& nums) {
    int len = nums.size();
    int low = 1;
    int high = len - 1;

    while (low < high) {
        int mid = low + (high - low) / 2;
        int cnt = 0;

        for (int i = 0; i < len; i++) {
            if (nums[i] <= mid) {
                cnt++;
            }
        }

        if (cnt <= mid) {
            low = mid + 1;
        }
        else {
            high = mid;
        }
    }

    return low;
}
```

## Analysis

- **Time Complexity**: `O(nlogn)`
- **Space Complexity**: `O(1)`

# Bit

This method converts all the numbers to **binary**. If we can get **each bit** of the repeated number, we can rebuild the repeated number.

Count the set bits of `[1,n]` and the array numbers separately. For each bit, if the count in `nums` is greater than the count in `[1,n]`, that bit belongs to the duplicate.

With extra `O(1)` space, without modifying the input.

```cpp
int findDuplicate_bit(vector<int>& nums) {
    int n = nums.size();
    int ans = 0;
    int bit_max = 31;

    while (((n - 1) >> bit_max) == 0) {
        bit_max -= 1;
    }

    for (int bit = 0; bit <= bit_max; ++bit) {
        int x = 0, y = 0;

        for (int i = 0; i < n; ++i) {
            if ((nums[i] & (1 << bit)) != 0) {
                x += 1;
            }

            if (i >= 1 && ((i & (1 << bit)) != 0)) {
                y += 1;
            }
        }

        if (x > y) {
            ans |= 1 << bit;
        }
    }

    return ans;
}
```

## Analysis

- **Time Complexity**: `O(nlogn)`
- **Space Complexity**: `O(1)`

# Fast Slow Pointers

This problem is the same idea as **142. Linked List Cycle II**. The key is to treat the input array as a linked list.

For example, with:

```text
nums = [1,3,4,2]
```

we can map:

```text
0 → 1 → 3 → 2 → 4
```

With a duplicate:

```text
nums = [1,3,4,2,2]
```

we get:

```text
0 → 1 → 3 → 2 → 4
              ↑   ↓
              └───┘
```

The duplicate creates a cycle. We can use Floyd's **Fast & Slow Pointer** algorithm to find the entrance of that cycle, which is the duplicate number.

With extra `O(1)` space, without modifying the input.

```cpp
int findDuplicate_fastSlow(vector<int>& nums) {
    int slow = 0;
    int fast = 0;

    do {
        slow = nums[slow];
        fast = nums[nums[fast]];
    } while (slow != fast);

    slow = 0;

    while (slow != fast) {
        slow = nums[slow];
        fast = nums[fast];
    }

    return slow;
}
```

## Analysis

- **Time Complexity**: `O(n)`
- **Space Complexity**: `O(1)`  