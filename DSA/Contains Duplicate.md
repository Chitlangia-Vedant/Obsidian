# [217. Contains Duplicate](https://leetcode.com/problems/contains-duplicate/)

Given an integer array `nums`, return `true` if any value appears **at least twice** in the array, and return `false` if every element is distinct.

**Example 1:**

**Input:** nums = [1,2,3,1]

**Output:** true

**Explanation:**

The element 1 occurs at the indices 0 and 3.

**Example 2:**

**Input:** nums = [1,2,3,4]

**Output:** false

**Explanation:**

All elements are distinct.

**Example 3:**

**Input:** nums = [1,1,1,3,3,4,3,2,4,2]

**Output:** true

**Constraints:**

- `1 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`

# Approach

If you check all possible pairs, we need two loops which is O(n2).

```
Input: nums = [1,2,3,1]
```

All possible pairs are

```csharp
[1,1][1,2][1,3][1,1] = 4
[2,1][2,2][2,3][2,1] = 4
[3,1][3,2][3,3][3,1] = 4
[1,1][1,2][1,3][1,1] = 4

total 16 pairs
```

We need to iterate input array `16` times which is `4^2`. `4` is the length of input array.

Time Complexity: O($n^2$)

---
# Solution 1 - Sorting

Simply, if we sort all numbers, we can solve this question easily, because all duplicate numbers will be adjacent numbers.

```csharp
[1,2,3,1]
↓
[1,1,2,3]
```

Then we iterate through the input array from index `1` and check previous index every time.

---
## Complexity

- Time complexity: O(nlogn)

- Space complexity: O(n)  
    It depends on language you use.

## CODE

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] == nums[i - 1]) {
                return true;
            }
        }
        
        return false;        
    }
};
```

## Step by Step Algorithm

1. **Sorting the List**:

```python
nums.sort()
```

- `nums.sort()`: This line sorts the list `nums` in ascending order. Sorting helps in simplifying the detection of duplicates because if any duplicates exist, they will be next to each other in the sorted list.

**Explanation**:

- For example, consider the list `nums = [3, 1, 4, 2, 2]`. After sorting, the list will become `nums = [1, 2, 2, 3, 4]`.
- This makes it easy to check for duplicates since any duplicate values will now be adjacent.

2. **Iterating through the List**:

```python
for i in range(1, len(nums)):
```

- `for i in range(1, len(nums))`: This starts a loop that iterates through the list starting from the second element (index 1) to the last element.

**Explanation**:

- The `range(1, len(nums))` generates indices from 1 to `len(nums) - 1`.
- For the sorted list `[1, 2, 2, 3, 4]`, `range(1, 5)` will generate the sequence `[1, 2, 3, 4]`.
- The loop variable `i` will take these values in each iteration.

3. **Checking for Duplicates**:

```python
if nums[i] == nums[i - 1]:
```

- `if nums[i] == nums[i - 1]`: This condition checks whether the current element (`nums[i]`) is the same as the previous element (`nums[i - 1]`).

**Explanation**:

- For example, with the sorted list `[1, 2, 2, 3, 4]`:
    - When `i = 1`, `nums[1]` (which is 2) is compared with `nums[0]` (which is 1). They are not equal.
    - When `i = 2`, `nums[2]` (which is 2) is compared with `nums[1]` (which is also 2). They are equal, indicating a duplicate.

4. **Returning True if Duplicate is Found**:

```python
return True
```

- `return True`: If the condition `nums[i] == nums[i - 1]` is true, it means a duplicate has been found, so the method immediately returns `True`.

**Explanation**:

- In the example above, when `i = 2`, the duplicate condition is met, so the method returns `True` without checking the rest of the list.

5. **Returning False if No Duplicates are Found**:

```python
return False
```

- `return False`: If the loop completes without finding any duplicates, the method returns `False`.

**Explanation**:

- If the loop completes all iterations and no duplicates are found (e.g., for the list `[1, 2, 3, 4, 5]`), the method returns `False`.

---

# Solution 2 - Set

In the second solution, we use `set`. Every time we find a new number, we will check `set`. And if we have the same number in `set`, we should return `True`.

```csharp
[1,2,3,1]
set = {}
```

```csharp
[1,2,3,1]
 ↑

There is not 1 in set. Add 1 to set.
set = {1}
```

```csharp
[1,2,3,1]
   ↑

There is not 2 in set. Add 2 to set.
set = {1,2}
```

```csharp
[1,2,3,1]
     ↑

There is not 3 in set. Add 3 to set.
set = {1,2,3}
```

```csharp
[1,2,3,1]
       ↑

There is 1 in set.
set = {1,2,3}
```

```python
return True
```

---
## Complexity

- Time complexity: O(n)

- Space complexity: O(n)  
    In the worst case, numbers are all unique.

## CODE

```python
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> numSet;

        for (int n : nums) {
            if (numSet.find(n) != numSet.end()) {
                return true;
            }
            numSet.insert(n);
        }
        
        return false;        
    }
};
```

## CODE (Mine)

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> m;
        for(int i:nums){
            if(m.contains(i)){
                return true;
            }
            m.insert(i);
        }
        return false;
    }
};
```
## Step by Step Algorithm

1. **Set Initialization**:

```python
num_set = set()
```

- `num_set = set()`: This line initializes an empty set named `num_set`. The set is used to store unique numbers as we iterate through the list.

**Explanation**:

- A set is chosen because it provides average (O(1)) time complexity for both insertions and membership checks, which makes it efficient for checking duplicates.

2. **Iterating through the List**:

```python
for n in nums:
```

- `for n in nums`: This line starts a loop that iterates over each element `n` in the list `nums`.

**Explanation**:

- This loop will go through each number in the list one by one. For example, if `nums = [1, 2, 3, 1]`, `n` will take on the values 1, 2, 3, and 1 in successive iterations.

3. **Checking for Duplicates**:

```python
if n in num_set:
```

- `if n in num_set`: This line checks if the current number `n` is already in the set `num_set`.

**Explanation**:

- If `n` is found in `num_set`, it means that this number has appeared before in the list, indicating a duplicate.

4. **Returning True if Duplicate is Found**:

```python
return True
```

- `return True`: If the condition `n in num_set` is true, the method immediately returns `True`.

**Explanation**:

- As soon as a duplicate is found, there's no need to continue checking the rest of the list, so the function returns `True` to indicate that a duplicate exists.

5. **Adding the Number to the Set**:

```python
num_set.add(n)
```

- `num_set.add(n)`: If `n` is not found in `num_set`, this line adds `n` to the set.

**Explanation**:

- This ensures that the set contains all unique numbers seen so far. For example, if `nums = [1, 2, 3, 1]`, after the first iteration, `num_set` will contain `{1}`, after the second iteration `{1, 2}`, and so on.

6. **Returning False if No Duplicates are Found**:

```python
return False
```

- `return False`: If the loop completes without finding any duplicates, the method returns `False`.

**Explanation**:

- If the loop completes all iterations and no duplicates are found (e.g., for the list `[1, 2, 3, 4]`), the method returns `False` to indicate that there are no duplicate elements in the list.

---

# Solution 3 - Check length

**If the length of the input array created by executing set is shorter than the original input array, it indicates that we have duplicate numbers.**

---
## Complexity

- Time complexity: O(n)

- Space complexity: O(n)

```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set<int> numSet(nums.begin(), nums.end());
        return numSet.size() < nums.size();        
    }
};
```

## Step by Step Algorithm

1. **Convert List to Set**:

```python
return True if len(set(nums)) < len(nums) else False
```

- `set(nums)`: This converts the list `nums` into a set. A set automatically removes any duplicate elements because sets only store unique values.

**Explanation**:

- For example, if `nums = [1, 2, 3, 2]`, converting it to a set would result in `{1, 2, 3}`.

2. **Calculate Length of Set**:
    
    - `len(set(nums))`: This calculates the number of unique elements in the list. It represents the size of the set.
    
    **Explanation**:
    
    - Continuing with the example above, `len(set(nums))` would be `3` because the set `{1, 2, 3}` contains three unique elements.
3. **Calculate Length of Original List**:
    
    - `len(nums)`: This calculates the number of elements in the original list.
    
    **Explanation**:
    
    - For the example, `len(nums)` would be `4` because the original list `[1, 2, 3, 2]` contains four elements.
4. **Compare Lengths and Return Result**:
    
    - `len(set(nums)) < len(nums)`: This compares the length of the set (unique elements) with the length of the original list. If the length of the set is less than the length of the list, it indicates that there are duplicate elements in the list.
    
    **Explanation**:
    
    - For the example, `len(set(nums))` is `3`, and `len(nums)` is `4`. Since `3 < 4`, it means there are duplicates in the list.
        
    - `return True if len(set(nums)) < len(nums) else False`: This conditional expression returns `True` if there are duplicates (i.e., if the length of the set is less than the length of the list). Otherwise, it returns `False`.
        
    
    **Explanation**:
    
    - For the example, since `len(set(nums)) < len(nums)` evaluates to `True`, the method returns `True`.```