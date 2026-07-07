https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters (LC Premium)
https://www.lintcode.com/problem/928/
# 928 · Longest Substring with At Most Two Distinct Characters

## Description

Given a string, find the length of the longest substring T that contains at most `2` distinct characters.

---
### Example 1

```
Input: “eceba”
Output: 3
Explanation:
T is "ece" which its length is 3.
```
### Example 2

```
Input: “aaa”
Output: 3
```

```cpp
class Solution {
public:
    /**
     * @param s: a string
     * @return: the length of the longest substring T that contains at most 2 distinct characters
     */
    int lengthOfLongestSubstringTwoDistinct(string &s) {
        // Write your code here
    }
};
```
# Understanding:

```
eceba
unique_chars <= 2

String       Unique Characters    Size

ec              2                    2
ece             2                    3 : MAX
eceb            3                    4
eceba           4                    5
```

# Solution:

```Java
class Solution 
{
    public int lengthOfLongestSubstringTwoDistinct(String s)    
    {
        int[] freq = new int[128];
        int left = 0, right = 0;
        int ans = 0, n = s.length();
        int uniqueCharCount = 0;
        int k = 2;

        while (right < n)
        {
            // right++: Expanding the Window: ALWAYS
            // Post increment freq = 0, Means CURRENT Freq is 0 - Unique
            if (freq[s.charAt(right++)]++ == 0)
            {
                uniqueCharCount++;

                while (uniqueCharCount > k)
                {
                    // left++: Shrinking the Window
                    // Shrink the Window only when there is repeating char and till freq == 0
                    // freq == 0, No More Reepating char
                    if (--freq[s.charAt(left++)] == 0)
                    {
                        uniqueCharCount--;
                    }
                }
            }
            ans = Math.max(ans, right-left);
        }

        return ans;
        
    }
}
```

https://algo.monster/liteproblems/159

TC: O(N)
SC: O(128)