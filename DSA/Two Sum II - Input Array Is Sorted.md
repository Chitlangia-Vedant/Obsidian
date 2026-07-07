# [167. Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)

Given a **1-indexed** array of integers `numbers` that is already **_sorted in non-decreasing order_**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where `1 <= index1 < index2 <= numbers.length`.

Return _the indices of the two numbers_ `index1` _and_ `index2`_, **each incremented by one,** as an integer array_ `[index1, index2]` _of length 2._

The tests are generated such that there is **exactly one solution**. You **may not** use the same element twice.

Your solution must use only constant extra space.

**Example 1:**

**Input:** numbers = [2,7,11,15], target = 9
**Output:** [1,2]
**Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

**Example 2:**

**Input:** numbers = [2,3,4], target = 6
**Output:** [1,3]
**Explanation:** The sum of 2 and 4 is 6. Therefore index1 = 1, index2 = 3. We return [1, 3].

**Example 3:**

**Input:** numbers = [-1,0], target = -1
**Output:** [1,2]
**Explanation:** The sum of -1 and 0 is -1. Therefore index1 = 1, index2 = 2. We return [1, 2].

**Constraints:**

- `2 <= numbers.length <= 3 * 104`
- `-1000 <= numbers[i] <= 1000`
- `numbers` is sorted in **non-decreasing order**.
- `-1000 <= target <= 1000`
- The tests are generated such that there is **exactly one solution**.

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        
    }
};
```

# Approach (Two Pointer Technique)

We place two pointers at the **extremes of the array**:

```sql
start → beginning of the array
end   → end of the array
```

Then we check the sum:

```sql
numbers[start] + numbers[end]
```

---

### Case 1: Sum equals target

```sql
numbers[start] + numbers[end] == target
```

We found the answer.

Return:

```sql
[start + 1, end + 1]
```

Why `+1`?

Because the problem requires **1-based indexing**.

---

### Case 2: Sum is smaller than target

```sql
numbers[start] + numbers[end] < target
```

We need a **larger sum**.

Since the array is sorted, we move:

```sql
start++
```

to increase the value.

---

### Case 3: Sum is larger than target

```sql
numbers[start] + numbers[end] > target
```

We need a **smaller sum**.

So we move:

```sql
end--
```

to reduce the value.

---

### Step-by-Step Example

```
numbers = [2,7,11,15]
target = 9
```

Start:

```sql
start = 0
end = 3
```

Check:

```
2 + 15 = 17 > 9
```

Move:

```sql
end--
```

---

Now:

```sql
start = 0
end = 2
```

Check:

```
2 + 11 = 13 > 9
```

Move:

```sql
end--
```

---

Now:

```sql
start = 0
end = 1
```

Check:

```
2 + 7 = 9
```

Target found.

Return:

```csharp
[1,2]
```

---

# Complexity Analysis

### Time Complexity

```lisp
O(n)
```

Each pointer moves at most once through the array.

---

### Space Complexity

```lisp
O(1)
```

No extra memory is used.

---

#  Code

```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {

        int start = 0;
        int end = numbers.length - 1;

        while(start < end){

            int sum = numbers[start] + numbers[end];

            if(sum == target){
                return new int[]{start + 1, end + 1};
            }
            else if(sum < target){
                start++;
            }
            else{
                end--;
            }
        }

        return new int[]{-1, -1};
    }
}
```

# CODE (Mine)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int i=0,j=numbers.size()-1;
        while(numbers[i]+numbers[j]!=target){
            if(numbers[i]+numbers[j]>target)
                j--;
            if(numbers[i]+numbers[j]<target)
                i++;
        }
        return {i+1,j+1};
    }
};
```