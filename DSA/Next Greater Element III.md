# [556. Next Greater Element III](https://leetcode.com/problems/next-greater-element-iii/)

Given a positive integer `n`, find _the smallest integer which has exactly the same digits existing in the integer_ `n` _and is greater in value than_ `n`. If no such positive integer exists, return `-1`.

**Note** that the returned integer should fit in **32-bit integer**, if there is a valid answer but it does not fit in **32-bit integer**, return `-1`.

**Example 1:**

**Input:** n = 12
**Output:** 21

**Example 2:**

**Input:** n = 21
**Output:** -1

**Constraints:**

- `1 <= n <= 231 - 1`
- 
```cpp
class Solution {
public:
    int nextGreaterElement(int n) {
        
    }
};
```

## Intuition

How our algorithm should work:

1. Start from the end and look for increasing pattern, it our case `7641`.
2. If it happen, that all number has increasing pattern, there is no bigger number with the same digits, so we can return `-1`.
3. Now, we need to find the first digit in our ending, which is less or equal to `digits[i-1]`: we have ending `5 7641` and we are looking for the next number with the same digits. What can go instead of `5`: it is `6`! Let us change these two digits, so we have `6 7541` now. Finally, we need to reverse last ditits to get `6 1457` as our ending.

---

## Core Idea (Same for all)

### Step 1: Find Pivot

```text
Find first index i from right such that s[i] < s[i+1]
```

---

### Step 2: If no pivot

```text
Number is fully descending → no answer → return -1
```

---

### Step 3: Suffix property

```text
s[i+1 ... end] is ALWAYS descending
```

---

### Step 4: Replace pivot

```text
Find just greater element than s[i]
```

---

### Step 5: Reverse suffix

```text
To get smallest possible number
```

# CODE(Mine)

```cpp
class Solution {
public:
    int nextGreaterElement(int n) {
        vector<int> digits = intToVec(n);
        for(int i=0;i<digits.size()-1;i++){
            if(digits[i+1]<digits[i]){
                int j;
                for(j=0;digits[i+1]>=digits[j];j++){}
                int temp=digits[j];
                digits[j]=digits[i+1];
                digits[i+1]=temp;
                reverse(digits.begin(),digits.begin()+i+1);
                return vecToInt(digits);           
            }
        }
        return -1;

    }
    vector<int> intToVec(int n){
        vector<int> digits;
        while(n>0){
            digits.push_back(n%10);
            n/=10;
        }
        return digits;
    }
    int vecToInt(vector<int> digits){
        int n=0;
        for(int i: std::views::reverse(digits)){
            if (n > INT_MAX / 10 ||(n == INT_MAX / 10 && i > INT_MAX % 10))
                return -1;
            n*=10;
            n+=i;
        }
        return n;
    }
};
```

**Complexity**: time complexity is `O(m)`, where `m` is number of digits in our number, space complexity `O(m)` as well.

# CODE

```cpp
class Solution {
public:
    int nextGreaterElement(int n) {
        // Convert number to string for digit manipulation
        string digits = to_string(n);
        int length = digits.size();
      
        // Step 1: Find the rightmost digit that is smaller than its next digit
        // This is the pivot point where we need to make a change
        int pivotIndex = length - 2;
        while (pivotIndex >= 0 && digits[pivotIndex] >= digits[pivotIndex + 1]) {
            pivotIndex--;
        }
      
        // If no such digit exists, the number is already the largest permutation
        if (pivotIndex < 0) {
            return -1;
        }
      
        // Step 2: Find the smallest digit to the right of pivot that is larger than pivot
        // This digit will be swapped with the pivot
        int swapIndex = length - 1;
        while (digits[pivotIndex] >= digits[swapIndex]) {
            swapIndex--;
        }
      
        // Step 3: Swap the pivot with the found digit
        swap(digits[pivotIndex], digits[swapIndex]);
      
        // Step 4: Reverse all digits after the pivot position to get the smallest permutation
        // This ensures we get the next greater element, not just any greater element
        reverse(digits.begin() + pivotIndex + 1, digits.end());
      
        // Convert back to number and check for integer overflow
        long result = stol(digits);
      
        // Return -1 if the result exceeds 32-bit integer range
        return result > INT_MAX ? -1 : result;
    }
};
```