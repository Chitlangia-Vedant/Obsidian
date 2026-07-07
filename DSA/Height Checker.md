# [1051. Height Checker](https://leetcode.com/problems/height-checker/)

A school is trying to take an annual photo of all the students. The students are asked to stand in a single file line in **non-decreasing order** by height. Let this ordering be represented by the integer array `expected` where `expected[i]` is the expected height of the `ith` student in line.

You are given an integer array `heights` representing the **current order** that the students are standing in. Each `heights[i]` is the height of the `ith` student in line (**0-indexed**).

Return _the **number of indices** where_ `heights[i] != expected[i]`.

**Example 1:**

**Input:** heights = [1,1,4,2,1,3]
**Output:** 3
**Explanation:** 
heights:  [1,1,4,2,1,3]
expected: [1,1,1,2,3,4]
Indices 2, 4, and 5 do not match.

**Example 2:**

**Input:** heights = [5,1,2,3,4]
**Output:** 5
**Explanation:**
heights:  [5,1,2,3,4]
expected: [1,2,3,4,5]
All indices do not match.

**Example 3:**

**Input:** heights = [1,2,3,4,5]
**Output:** 0
**Explanation:**
heights:  [1,2,3,4,5]
expected: [1,2,3,4,5]
All indices match.

**Constraints:**

- `1 <= heights.length <= 100`
- `1 <= heights[i] <= 100`

```cpp
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        
    }
};
```

# Solution 1

1. Copy the array `height` in new array `expected`
2. Iterate through `height` and `expected` and for every where `height[i]` and `expected[i]` are not equal increase `count`

## CODE (Mine)

```cpp
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        vector<int> expected=heights;
        sort(expected.begin(),expected.end());
        int count=0;
        for(int i=0;i<heights.size();i++){
            if(heights[i]!=expected[i])
                count++;
        }
        return count;
    }
};
```

# Solution 2

The heights range from 1 to 100. We can use this to construct a solution with time-complexity O(n).

We can create an array(or map) of size 101 to store the occurrence of heights.  
![image](https://assets.leetcode.com/users/images/02076d14-0bc0-43bc-aa27-4f87cee957f4_1624028204.0069916.jpeg)

Now we will traverse the **array heights** and **array count** and compare the **index of count** with **value of heights**.

## CODE

```cpp
    int heightChecker(vector<int>& heights) {
        int mismatch = 0; //count the number of mismatches
        vector<int> count(101, 0); // store the count of heights

        for(int i = 0; i < heights.size(); i++) count[heights[i]]++;

        int i = 1, j = 0; 
        while(i < 101){
			// check value of count
            if(count[i] == 0){
                i++;
            }
            else{
				// compare the index of count with value of height
                if(i != heights[j]) mismatch++;
                j++; count[i]--;
            }
        }

        return mismatch;
    }
```