# [268. Missing Number](https://leetcode.com/problems/missing-number/)

Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._

**Example 1:**

**Input:** nums = [3,0,1]

**Output:** 2

**Explanation:**

`n = 3` since there are 3 numbers, so all numbers are in the range `[0,3]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 2:**

**Input:** nums = [0,1]

**Output:** 2

**Explanation:**

`n = 2` since there are 2 numbers, so all numbers are in the range `[0,2]`. 2 is the missing number in the range since it does not appear in `nums`.

**Example 3:**

**Input:** nums = [9,6,4,2,3,5,7,0,1]

**Output:** 8

**Explanation:**

`n = 9` since there are 9 numbers, so all numbers are in the range `[0,9]`. 8 is the missing number in the range since it does not appear in `nums`.

```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        
    }
};
```

# Understanding:

```
(a,b): Exclusive: Open Interval

[a,b]: Inclusive: Closed Interval

Total Values = b-a+1

N: Size of Array
Range: [0, N]
Total Expected Value = N-0+1 = N + 1
Given Values: N
1 Missing Number -------> Find that
```

# Solution:


## (1) Brute Force:

Two Nested Loops:

Expected: [0,1,2,3,4.......N]
Current: 1 Value Missing

Outer Loop: 0 to N (N+1 Times)
Inner Loop: All Array

```
Check if i == arr[i]
```

TC: O(N^2)
SC: O(1)

## (2) Sorting and Check
- Binary Search

TC: O(NlogN) + O(logN)
SC: O(1)

## (3) XOR Based Solution:

[Bits Manipulation](DSA/Theory/Bits Manipulation)

```
[3,0,1]: Missing: 2

Total Expected Valie: N + 1
Current Values: N
1 Missinhg Value

Do XOR of arr[i] with [0......N]

(Arr Values) ^ [0.......N] = Missing Number

(3^0^1) ^ (0^1^2^3) = 2 - Missing Number
```

### CODE:

```cpp
 
    public int missingNumber(int[] nums) 
    {
        int ans = 0, i = 0;
        
        
//        (3^0^1) ^ (0^1^2^3) = 2 - ANS

  //      (Arr Values) ^ (0......N) = Missing Number
        
           
        for (i=0; i<nums.length; i++) // i = 0 to N-1
            ans ^= i^nums[i]; // (Arr Values) ^ [0......N]
        
        return ans^i; // i = N
    }
```

TC: O(N)
SC: O(1)

## (4)Sum and Formula:

Expected Values: [0,1,2,......N]
Expected Sum: 0 + 1 + 2 ...... N
			= 1 + 2 + ....... N
			= N * (N+1)/2


Missing Number = Expected Sum - Array Sum

### CODE
```cpp
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n=nums.size();
        int asum=(n*(n+1))/2;
        int bsum=0;
        for (int i=0;i<n;i++){
            bsum+=nums[i];
        }
        return asum-bsum;
    }
};
```

TC: O(N)
SC: O(1)

### **Constraints:**

```
n == nums.length
1 <= n <= 104
0 <= nums[i] <= n
All the numbers of nums are unique.

expected_sum = 10^4*10^4 = 10^8
curr_sum = 10^4*10^4 = 10^8

Range of int: -2*10^9 to 2*10^9
```

### **Change:**

```
1 <= n <= 10^6
0 <= nums[i] <= n

expected_sum = 10^6*10^6 = 10^12
curr_sum = 10^6*10^6 = 10^12

Range of int: -2*10^9 to 2*10^9

Now, we cannot define these values as int ----> Overflow
```

### Solutions:

#### (1) Define them as long
int: 4 Bytes
long: 8 bytes
#### (2) Keeping it as int and still passing

**Root Cause:** 
Addition or Multiplication -----> Overflow

**Intuition:**
Can we still calculate "sum" without using addition and multiplication
N * (N+1)/2 -----> Overflow
Rely upon subtraction or Division -----> Solve the problem of overflow


**Thoughts/Working Solution:**
result = expected_sum - curr_sum;
Requirement is NOT to calculate expected_sum or curr_sum
It is to find DIFFERENCE of these two: "expected_sum - curr_sum"

#### CODE:
IP: [3,0,1]
N = 3
OP: 2

```cpp
public int missingNumber(int[] nums) 
{
	int i=0, n= nums.length;
	int sum = n; // 3

	for (i=0; i<n; i++)
		sum += i - nums[i]; // Will NOT Overflow

	 sum += 0 - nums[0] = 3-3 = 0
	 sum += 1 - nums[1] = 1-0 = 1
	 sum += 2 - nums[2] = 1+2-1 = 2: ANS

	return sum; // 2
}
```

TC: O(N)
SC: O(1)

```
Expected Values: [0,1,2.......N]
Current Values: [0,1,2.......N] - 1 Value Missing

Example:

Expected = [0,1,2,3]
Current = [0,1,3]
OP: 2
```

I DO NOT NEED Sum of Expected or Sum of Current, I just need the DIFFERENCE

```
[0,1,2,3] - [0,1,3] = 2
```