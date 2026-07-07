# [496. Next Greater Element I](https://leetcode.com/problems/next-greater-element-i/)

The **next greater element** of some element `x` in an array is the **first greater** element that is **to the right** of `x` in the same array.

You are given two **distinct 0-indexed** integer arrays `nums1` and `nums2`, where `nums1` is a subset of `nums2`.

For each `0 <= i < nums1.length`, find the index `j` such that `nums1[i] == nums2[j]` and determine the **next greater element** of `nums2[j]` in `nums2`. If there is no next greater element, then the answer for this query is `-1`.

Return _an array_ `ans` _of length_ `nums1.length` _such that_ `ans[i]` _is the **next greater element** as described above._

**Example 1:**

**Input:** nums1 = [4,1,2], nums2 = [1,3,4,2]
**Output:** [-1,3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 4 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.
- 1 is underlined in nums2 = [1,3,4,2]. The next greater element is 3.
- 2 is underlined in nums2 = [1,3,4,2]. There is no next greater element, so the answer is -1.

**Example 2:**

**Input:** nums1 = [2,4], nums2 = [1,2,3,4]
**Output:** [3,-1]
**Explanation:** The next greater element for each value of nums1 is as follows:
- 2 is underlined in nums2 = [1,2,3,4]. The next greater element is 3.
- 4 is underlined in nums2 = [1,2,3,4]. There is no next greater element, so the answer is -1.

**Constraints:**

- `1 <= nums1.length <= nums2.length <= 1000`
- `0 <= nums1[i], nums2[i] <= 104`
- All integers in `nums1` and `nums2` are **unique**.
- All the integers of `nums1` also appear in `nums2`.

**Follow up:** Could you find an `O(nums1.length + nums2.length)` solution?

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        
    }
};
```

# Solution 1: Brute Force
1. Process each element of nums1
2. For each element, identify its position in nums2
3. From that position onward, find the first greater element

## Complexity
- Time complexity: O(m∗n)
- Space complexity: O(1) except `res`.

## CODE 

```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res = []

        for target in nums1:
            ng = -1
            target_found = False

            for num in nums2:
                if num == target:
                    target_found = True
                elif target_found:
                    if num > target:
                        ng = num
                        break
            
            res.append(ng)
        
        return res
```
# Solution 2

## CODE (Mine)

1. Store the `nums1` value as key and respective index as value in a Map `mp`
2. Iterate the `nums2` in reverse and for every `nums1` value in `nums2` calculate the nge
	- [[Next Greater Element]]

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> s;
        unordered_map<int,int> mp;
        for(int i=0;i<nums1.size();i++){
            mp[nums1[i]]=i;
        }
        for(auto &i : views::reverse(nums2)){
            if(mp.contains(i)){
                while (!s.empty() && s.top()<=i)
			        s.pop();
                if (s.empty())
			        nums1[mp[i]] = -1;
                else
			        nums1[mp[i]] = s.top();
            }
            s.push(i);
        }
        return nums1;
    }
};
```

# Solution 3

```
nums2 = [2,1,3]
st = []
```

When stack is empty, we just push current number into stack.

```
nums2 = [2,1,3]
         ↑
st = [2]
```

Next

```
nums2 = [2,1,3]
           ↑
st = [2]
```

We have 2 in stack. In that case, compare current number with top of stack and if current number is greater than to of stack, current number is next greater element for top of stack.

```
2 vs 1
```

In this case, top of stack is greater than current number, so just push current number into stack.

```
st = [2,1]
```

Next

```
nums2 = [2,1,3]
             ↑
st = [2,1]
```

We found 3, compare 3 with 1. 3 is greater than 1, that means 3 is next greater element for 1. In that case, pop top of stack and keep 1 and 3 combination with hashmap.

```
{1:3}
st = [2]
```

We continue to compare 3 with 2. 3 is greater than 2, so 3 is next greater element for 2.

```
{1:3, 2:3}
st = []
```

Now stack is empty. Push 3 into stack.

```
{1:3, 2:3}
st = [3]
```

Then finish iteration. We have to return a result like this.

```sql
[ 3,1,2](= num1)
  ↓ ↓ ↓
[-1,3,3](= result)
```

So iterate through nums1 and find numbers in hashmap which has next greater elements. For 3, we don't have key. In that case, return -1.

The stack has numbers with descending order for this question. Sometimes we need stack with ascending order. We call this kind of stack monotonic stack.

## Complexity

- Time complexity: O(m+n)  
    m is length of nums2  
    n is length of nums1
    
- Space complexity: O(m) execpt returned array.

## CODE

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> ng;
        stack<int> st;

        for (int num : nums2) {
            while (!st.empty() && st.top() < num) {
                ng[st.top()] = num;
                st.pop();
            }
            st.push(num);
        }

        vector<int> res;
        for (int num : nums1) {
            res.push_back(ng.count(num) ? ng[num] : -1);
        }
        return res;        
    }
};
```