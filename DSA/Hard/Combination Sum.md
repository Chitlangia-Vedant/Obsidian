# [39. Combination Sum](https://leetcode.com/problems/combination-sum/)

Given an array of **distinct** integers `candidates` and a target integer `target`, return _a list of all **unique combinations** of_ `candidates` _where the chosen numbers sum to_ `target`_._ You may return the combinations in **any order**.

The **same** number may be chosen from `candidates` an **unlimited number of times**. Two combinations are unique if the frequency of at least one of the chosen numbers is different.

The test cases are generated such that the number of unique combinations that sum up to `target` is less than `150` combinations for the given input.

**Example 1:**

**Input:** candidates = [2,3,6,7], target = 7
**Output:** `[[2,2,3],[7]]`
**Explanation:**
2 and 3 are candidates, and 2 + 2 + 3 = 7. Note that 2 can be used multiple times.
7 is a candidate, and 7 = 7.
These are the only two combinations.

**Example 2:**

**Input:** candidates = [2,3,5], target = 8
**Output:** `[[2,2,2,2],[2,3,3],[3,5]]`

**Example 3:**

**Input:** candidates = [2], target = 1
**Output:** []

**Constraints:**

- `1 <= candidates.length <= 30`
- `2 <= candidates[i] <= 40`
- All elements of `candidates` are **distinct**.
- `1 <= target <= 40`

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        
    }
};
```

# Solution:

## (1) Sort the Array Initially BEFORE Applying Backtracking
- To get Sorted Permutations

Backtracking in a Sorted Array -----> Sorted Permutations (idx+1)


## (2) Unique Values: Set


## (3) Apply Backtracking:

- Case-1:
	if target == 0:
		- Combination Found
		- Add that to your ans (List of Combinations)
- Case-2:
	if target < 0:
		- curr_sum > target
	- Ignore that case, Dont add to ans (List of Combinations)
- Case-3:
	if target > 0:
		- curr_sum < target
		- Insert the present array and backtrack:
		select = target -> target - arr[i]
		not_select = target -> target: NO CHANGE

# CODE:

```Java
class Solution {
public List<List<Integer>> combinationSum(int[] candidates, int target) {
    List<List<Integer>> ans = new ArrayList<>(); //2 D Array
    Arrays.sort(candidates);
    backtrack(ans, new ArrayList<Integer>(), candidates, target, 0);
    return ans;
}

private void backtrack(List<List<Integer>> ans, List<Integer> tempList, int[] arr, int target, int start)
{
    if (target < 0)  // target < 0
        return; // no solution 

    else if (target == 0)  // Found Combination
        ans.add(new ArrayList<>(tempList)); // ans.add([2 2 3])

    else // Apply Backtracking
    {
        for (int i = start; i < arr.length; i++) 
        { 
            tempList.add(arr[i]);
            backtrack(ans, tempList, arr, target-arr[i], i);
            tempList.remove(tempList.size()-1);
        } 
    }

}
}
```

TC: O(2^N)
SC: O(N) - Recursion Stack

# CODE (Mine)

```cpp
class Solution {
public:
    vector<int> candidates;
    vector<vector<int>> ans;
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        this->candidates=candidates;
        vector<int> combination={};
        solve(0,target,combination);
        
        return ans;
    }
    void solve(int i,int target, vector<int> combination){
        if(target==0){
            ans.push_back(combination);
            return;
        }
        if(target<0||i>=candidates.size()){
            return;
        }
        solve(i+1,target,combination);
        combination.push_back(candidates[i]);
        solve(i,target-candidates[i],combination);
    }
};
```