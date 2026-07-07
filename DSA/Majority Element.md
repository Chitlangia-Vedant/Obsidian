# [169. Majority Element](https://leetcode.com/problems/majority-element/)

Given an array `nums` of size `n`, return _the majority element_.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example 1:**

**Input:** nums = [3,2,3]
**Output:** 3

**Example 2:**

**Input:** nums = [2,2,1,1,1,2,2]
**Output:** 2

**Constraints:**

- `n == nums.length`
- `1 <= n <= 5 * 104`
- `-109 <= nums[i] <= 109`
- The input is generated such that a majority element will exist in the array.

**Follow-up:** Could you solve the problem in linear time and in `O(1)` space?

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        
    }
};
```

# Understanding:

```
[1 1 1 1 2 3 4 5]
Majority Element = ?

- INVALID TEST CASE

N = 8
N/2 = 4

Majority Element: freq > N/2

> N/2: Correct
>=N/2: Incorrect
```

Can there be more than 1 majority element?: NO

Solution:
(1) Brute Force: Check freq > N/2: TC: O(N^2)
(2) Map: freq > N/2 : TC: O(N)
(3) Sort and check value at arr[N/2]: O(NlogN)
(4) Partial Sorting
(5) Bayer Moore Algo: O(N)
## (1) Brute Force:

### CODE
```cpp
int freq = 0;

for (i=0; i<n; i++)
{
	freq = 0;
	for (j=i; j<n; j++)
	{
		if (arr[i] == arr[j])
			++freq;
	}

	if (freq > n/2)
		return arr[i];
}
```

TC: O(N^2)
SC: O(1)

## (2) Map

Key: Element
Value: Frequency

Frequency > N/2, Ans = Element

TC: O(N)
SC: O(N)


### CODE:

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) 
    {
        // Approach -1: Using Map
        // T - O(N), S- O(N)
        
        unordered_map<int, int> counter;
        int i=0, n = nums.size();
        
        /*
        
        for (int i=0; i<n; i++) // O(N)
            counter[nums[i]]++;
        
        for (auto it: counter) // O(Size of Map)
            if (it.second > n/2)
                return it.first;
        
        */
            
        for (int val: nums)  // O(N)
        {
            if (++counter[val] > n/2)
                return val;
        }
        
        return 0;
                
    }
};
```

## (3) Sorting:

nums = [2 2 1 1 1 2 2]
OP: 2

After Sorting:
nums = [1 1 1 2 2 2 2]

Trick:
After Sorting, at Index N/2: Majority Element
Valid: Freq of Majority Element > N/2

### CODE:

```cpp
    int majorityElement(vector<int>& nums) 
    {
        sort(nums.begin(), nums.end());
        return nums[nums.size()/2];
    }
```

TC: O(NlogN)
SC: O(1)

## (4) Partial Sorting (Only sort till N/2)

nums = [2 2 1 1 1 2 2]
OP: 2

After Sorting:
nums = [1 1 1 2 2 2 2]


Trick:
After Sorting, at Index N/2: Majority Element
Valid: Freq of Majority Element > N/2


## (5) Optimised - Bayer Moore Algo
https://www.cs.utexas.edu/~moore/best-ideas/mjrty/

We will sweep down the sequence starting at the pointer position shown above.

As we sweep we maintain a pair consisting of a current candidate and a counter. Initially, the current candidate is unknown and the counter is 0.

When we move the pointer forward over an element e:

If the counter is 0, we set the current candidate to e and we set the counter to 1.
If the counter is not 0, we increment or decrement the counter according to whether e is the current candidate.
When we are done, the current candidate is the majority element, if there is a majority.

### TRICK:

(1) if count == 0
	count++

(2) else
Two Cases:
(A) Same Element -> ++count;
(B) Diff Element -> count--;

return major;

### CODE:

```Java
class Solution {
    public int majorityElement(int[] nums) 
    {
        // Theory: https://www.cs.utexas.edu/~moore/best-ideas/mjrty/
        // Bayer-Moore Algo
        // T - O(N), S- O(1)

        // IP: [3 2 3]
        // OP: 3
  
        int major = nums[0], count = 1;  // major : 3
        int i=1;
        int n = nums.length; // n = 3
        
        for (i=1; i<n; i++) // i =1, i = 2
        {
            // New Element -> Increase frequency from 0 -> 1
            if (count == 0)
            {
                ++count; // count = 1
                major = nums[i]; // major = nums[2] = 3
            }
            
            // Same Element -> Increase Frequency By 1
            else if (nums[i] == major) // nums[1] =2, major = 3
                ++count;
            
            // Different Element -> Decrease Frequency By 1
            else
                --count; // count = 0
        }
        
        return major; // 3 - ANS
        
    }
}
```



TC: O(N)
SC: O(1)
