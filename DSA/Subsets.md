# [78. Subsets](https://leetcode.com/problems/subsets/)

Given an integer array `nums` of **unique** elements, return _all possible_ _subsets_ _(the power set)_.

The solution set **must not** contain duplicate subsets. Return the solution in **any order**.

**Example 1:**

**Input:** nums = [1,2,3]
**Output:** [[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]

**Example 2:**

**Input:** nums = [0]
**Output:** [[],[0]]

**Constraints:**

- `1 <= nums.length <= 10`
- `-10 <= nums[i] <= 10`
- All the numbers of `nums` are **unique**.

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        
    }
};
```

# Solution 1

To create subsets, point is that we have two choices for each number.

---

⭐️ Points

- Include current number
- Not include current number

---

This picture is an example with input array `[1,2,3]`

![スクリーンショット 2024-04-30 1.56.40.png](https://assets.leetcode.com/users/images/91e72321-e214-49db-8a46-43d02db5103c_1714409832.6692474.png)

First of all, we have two choices for `1`. We can create two subsets.

```csharp
[1] or []
```

Next, we have two choices for `2`. We need to think about each pattern (= `[1] or []`).

For `[1]` case, we can create subsets

```csharp
[1,2] or [1]

[1,2] is including 2 
[1] is not including 2 
```

For `[]` case, we can create subsets

```csharp
[2] or []

[2] is including 2 
[] is not including 2 
```

We have 4 subsets so far.

```csharp
[1,2], [1], [2], []
```

We do the same thing about `3` with all the 4 patterns above. In the end, we can create and return

```csharp
For [1,2],
[1,2,3]
[1,2]

For [1],
[1,3]
[1]

For [2],
[2,3]
[2]

For [],
[3]
[]
```

## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<int>> res;
    vector<vector<int>> subsets(vector<int>& nums) {
        f(nums,0,vector<int>{});
        return res;
    }
    
    void f(const std::vector<int>& nums,int i,vector<int> subset){
        if(i==nums.size()) 
        {    
            res.push_back(subset);
            return;
        }
        f(nums,i+1,subset);
        subset.push_back(nums[i]);
        f(nums,i+1,subset);
        return;
    }
};
```

# Solution 2:Bit Manipulation:

- Use bit manipulation to generate all possible combinations.
- For a set with n elements, there are 2^n possible subsets.
- Iterate from 0 to 2^n - 1 and for each integer, consider the set bits as indices of elements to include in the subset.
- This approach is efficient in terms of time complexity, as it avoids recursion and extra space.

## CODE

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        int n = nums.size();
        for (int i = 0; i < (1 << n); i++) {
            vector<int> subset;
            for (int j = 0; j < n; j++) {
                if ((i & (1 << j)) > 0) {
                    subset.push_back(nums[j]);
                }
            }
            result.push_back(subset);
        }
        return result;
    }
};
```

## Step-by-Step Walkthrough:

INPUT: `[1,2,3]`

1. Initially, the `result` list is empty.
2. We iterate over all possible subsets using bit manipulation. Since there are 3 elements in `nums`, there will be `2^3 = 8` subsets.
3. We start the iteration from `i = 0` to `i = 7`, inclusive.
4. For each value of `i`, we check the binary representation of `i` to determine which elements to include in the subset.
5. For `i = 0`, binary representation is `000`. It means the empty subset, so we add `[]` to the `result`.
6. For `i = 1`, binary representation is `001`. It means including only the third element (`nums[2] = 3`) in the subset. So, we add `[3]` to the `result`.
7. For `i = 2`, binary representation is `010`. It means including only the second element (`nums[1] = 2`) in the subset. So, we add `[2]` to the `result`.
8. For `i = 3`, binary representation is `011`. It means including the second and third elements (`nums[1] = 2` and `nums[2] = 3`) in the subset. So, we add `[2, 3]` to the `result`.
9. For `i = 4`, binary representation is `100`. It means including only the first element (`nums[0] = 1`) in the subset. So, we add `[1]` to the `result`.
10. For `i = 5`, binary representation is `101`. It means including the first and third elements (`nums[0] = 1` and `nums[2] = 3`) in the subset. So, we add `[1, 3]` to the `result`.
11. For `i = 6`, binary representation is `110`. It means including the first and second elements (`nums[0] = 1` and `nums[1] = 2`) in the subset. So, we add `[1, 2]` to the `result`.
12. For `i = 7`, binary representation is `111`. It means including all elements (`nums[0] = 1`, `nums[1] = 2`, and `nums[2] = 3`) in the subset. So, we add `[1, 2, 3]` to the `result`.
13. After iterating through all possible subsets, the `result` contains all subsets of `[1, 2, 3]`, including the empty subset, which is `[[], [3], [2], [2, 3], [1], [1, 3], [1, 2], [1, 2, 3]]`.

# Solution 3: Iterative Approach (Generating All Subsets):

- Start with an empty subset and gradually build it.
- Iterate through each element in the given set.
- For each element, clone the existing subsets, add the current element to each cloned subset, and then add these subsets to the result.
- This approach iterates through the set only once and builds subsets incrementally.

# Code:

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result = {{}}; // Add the empty subset
        for (int num : nums) {
            int size = result.size();
            for (int i = 0; i < size; i++) {
                vector<int> subset = result[i];
                subset.push_back(num);
                result.push_back(subset);
            }
        }
        return result;
    }
};
```

## Step-by-Step Walkthrough:

 Input: `nums = [1, 2, 3]`.

1. Initially, the `result` list contains only the empty subset, `[]`.
2. We iterate over each element in `nums`.
3. For the first element `1`:
    - We clone the existing subsets in `result`, which is currently `[]`.
    - We add `1` to each cloned subset and add them back to `result`. So, `result = [[], [1]]`.
4. For the second element `2`:
    - We clone the existing subsets in `result`, which are `[[], [1]]`.
    - We add `2` to each cloned subset and add them back to `result`. So, `result = [[], [1], [2], [1, 2]]`.
5. For the third element `3`:
    - We clone the existing subsets in `result`, which are `[[], [1], [2], [1, 2]]`.
    - We add `3` to each cloned subset and add them back to `result`. So, `result = [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]`.
6. After iterating through all elements in `nums`, the `result` contains all subsets of `[1, 2, 3]`, including the empty subset.