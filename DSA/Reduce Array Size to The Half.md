# [1338. Reduce Array Size to The Half](https://leetcode.com/problems/reduce-array-size-to-the-half/)

You are given an integer array `arr`. You can choose a set of integers and remove all the occurrences of these integers in the array.

Return _the minimum size of the set so that **at least** half of the integers of the array are removed_.

**Example 1:**

**Input:** arr = [3,3,3,3,5,5,5,2,2,7]
**Output:** 2
**Explanation:** Choosing {3,7} will make the new array [5,5,5,2,2] which has size 5 (i.e equal to half of the size of the old array).
Possible sets of size 2 are {3,5},{3,2},{5,2}.
Choosing set {2,7} is not possible as it will make the new array [3,3,3,3,5,5,5] which has a size greater than half of the size of the old array.

**Example 2:**

**Input:** arr = [7,7,7,7,7,7]
**Output:** 1
**Explanation:** The only possible set you can choose is {7}. This will make the new array empty.

**Constraints:**

- `2 <= arr.length <= 105`
- `arr.length` is even.
- `1 <= arr[i] <= 105`

```cpp
class Solution {
public:
    int minSetSize(vector<int>& arr) {
        
    }
};
```

# Solution

1. Calculate the frequency of each elements
2. Sort the element by the frequency
3. Remove the elements in descending order until the sum of frequency is smaller than the n/2

```cpp
class Solution {
public:
    int minSetSize(vector<int>& arr) {
        unordered_map <int,int> mp;
        for(auto &i:arr){
            mp[i]++;
        }
        std::vector<std::pair<int, int>> vec(mp.begin(), mp.end());
        std::sort(vec.begin(), vec.end(), [](pair<int,int> a, pair<int,int> b)-> bool{return a.second>b.second;});
        int count=0;
        int res=0;
        for(auto &i:vec){
            res++;
            count+=i.second;
            if(count>=arr.size()/2)
                return res;
        }
        return res;
    }
};
```