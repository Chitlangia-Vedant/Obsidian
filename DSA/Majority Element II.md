# [229. Majority Element II](https://leetcode.com/problems/majority-element-ii/)

Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example 1:**

**Input:** nums = [3,2,3]
**Output:** [3]

**Example 2:**

**Input:** nums = [1]
**Output:** [1]

**Example 3:**

**Input:** nums = [1,2]
**Output:** [1,2]

**Constraints:**

- `1 <= nums.length <= 5 * 104`
- `-109 <= nums[i] <= 109`

**Follow up:** Could you solve the problem in linear time and in `O(1)` space?

# Solution 1

## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        unordered_map<int,int> m;
        for(int i:nums){
            m[i]++;
        }
        vector<int> ans;
        for(auto i:m){
            if(i.second>nums.size()/3)
                ans.push_back(i.first);
        }
        return ans;
    }
};
```

# Solution 2
## Intuition

The problem requires us to find all elements that appear more than ⌊n/3⌋ times. By mathematical logic, an array can have at most **two** such elements (since 3×(more than 1/3)>1).

This allows us to apply **Boyer-Moore’s Voting Algorithm** using two candidates. We can think of it as a multi-party election where a new distinct number can cancel out one vote from two different leading candidates simultaneously.

## Approach

The algorithm works in two clean phases:

1. **Candidate Selection (Phase 1):**
    
    - We maintain two potential candidates (`el1`, `el2`) and their corresponding vote counters (`cnt1`, `cnt2`).
    - As we traverse the array, if a counter drops to `0`, we seize the current number as a new candidate.
    - If the current number matches an existing candidate, we increment its counter.
    - If it matches neither, it "challenges" both, so we decrement both counters.
2. **Verification (Phase 2):**
    
    - Since Boyer-Moore only gives us the top two contenders (not a guarantee that they actually cross the threshold), we reset the counters to `0`.
    - We run a second pass through the array to find the exact frequency of `el1` and `el2`.
    - If their actual count is strictly greater than ⌊n/3⌋, we add them to the final result.

## Complexity

- **Time complexity:** O(n)  
    We traverse the array exactly two times. The first pass determines the two potential candidates, and the second pass verifies their actual frequencies. Both passes run in linear time.
    
- **Space complexity:** O(1)  
    The algorithm uses a constant amount of extra space (`cnt1`, `cnt2`, `el1`, `el2`) regardless of the input array size. The memory used for the output vector does not count toward the extra space complexity.
    

## Code

```cpp
class Solution {
public:
    vector<int> majorityElement(vector<int>& nums) {
        int cnt1=0,cnt2=0,n=nums.size(),el1=INT_MIN,el2=INT_MIN,mini=n/3;
        vector<int> ans;

        for(int i=0;i<n;i++){
            if(el2!=nums[i]&&cnt1==0){
                el1=nums[i];
                cnt1++;
            }
            else if(el1!=nums[i]&&cnt2==0){
                el2=nums[i];
                cnt2++;
            }
            else if(el1==nums[i])cnt1++;
            else if(el2==nums[i])cnt2++;
            else{
                cnt1--;
                cnt2--;
            }
        }
        cnt1=0,cnt2=0;
        for(int i=0;i<n;i++){
            if(el1==nums[i])cnt1++;
            else if(el2==nums[i])cnt2++;
        }
        if(cnt1>mini)ans.push_back(el1);
        if(cnt2>mini)ans.push_back(el2);
        return ans;
    }
};
```