# [46. Permutations](https://leetcode.com/problems/permutations/)

Given an array `nums` of distinct integers, return all the possible permutations. You can return the answer in **any order**.

**Example 1:**

**Input:** nums = `[1,2,3]`
**Output:** `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

**Example 2:**

**Input:** nums = `[0,1]`
**Output:** `[[0,1],[1,0]]`

**Example 3:**

**Input:** nums = [1]
**Output:** [[1]]

**Constraints:**

- `1 <= nums.length <= 6`
- `-10 <= nums[i] <= 10`
- All the integers of `nums` are **unique**.

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        
    }
};
```

# [47. Permutations II](https://leetcode.com/problems/permutations-ii/)

Given a collection of numbers, `nums`, that might contain duplicates, return _all possible unique permutations **in any order**._

**Example 1:**

**Input:** nums = [1,1,2]
**Output:**
[[1,1,2],
 [1,2,1],
 [2,1,1]]

**Example 2:**

**Input:** nums = [1,2,3]
**Output:** [[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]

**Constraints:**

- `1 <= nums.length <= 8`
- `-10 <= nums[i] <= 10`

```cpp
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        
    }
};
```
# Solution 

```cpp
class Solution {
public:
    vector<vector<int>> res;

    void f(unordered_map<int,int>& mp,vector<int> combination){
        bool map_empty=true;
        for(auto &x:mp){
            if(x.second>0){
                x.second--;
                combination.push_back(x.first);
                f(mp, combination);
                combination.pop_back();
                x.second++;
                map_empty=false;
            } 
        }
        if(map_empty){
            res.push_back(combination);
        }
        return;
    }
    vector<vector<int>> permute(vector<int>& nums) {
        unordered_map<int,int> mp;
        for(int &x:nums){
            mp[x]++;
        }
        for(auto &x:mp){
            if(x.second>0){
                x.second--;
                f(mp,vector<int>{x.first});
                x.second++;
            }       
        }
        return res;        
    }
};
```