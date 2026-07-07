https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/ (LC premium)
https://www.lintcode.com/problem/386
# 386 · Longest Substring with At Most K Distinct Characters

Description

Given a string _S_, find the length of the longest substring _T_ that contains at most k distinct characters.

Example

**Example 1:**

```
Input: S = "eceba" and k = 3
Output: 4
Explanation: T = "eceb"
```

**Example 2:**

```
Input: S = "WORLD" and k = 4
Output: 4
Explanation: T = "WORL" or "ORLD"
```

Challenge

O(n) time

```cpp
class Solution {
public:
    /**
     * @param s: A string
     * @param k: An integer
     * @return: An integer
     */
    int lengthOfLongestSubstringKDistinct(string &s, int k) {
        // write your code here
    }
};
```

# Understanding:

eceba, K = 2
unique_chars <\= K

```
String       Unique Characters    Size

ec              2                    2
ece             2                    3 : MAX
eceb            3                    4
eceba           4                    5
```

aa, K = 1
unique_chars <\= K

```
String       Unique Characters    Size

a              1                    1
aa             1                    2 : MAX
```

# Solution:

At Most K Unique Characters: Map.Freq Array

## Approach:

Sliding Window + Map/Freq Array


(1) Map/Freq Array: Keep Track of count of each char and uniqueCounter <= K
- If Unique, Expand Window Size, uniqueCounter++
- If Repeated, Decrease Window Size, uniqueCounter--

(2) Maintain Window Size using left and right pointer

# CODE:

```Java
class Solution 
{
    public int lengthOfLongestSubstringKDistinct(String s, int k) 
    {
        int[] freq = new int[128];
        int left = 0, right = 0;
        int ans = 0, n = s.length();
        int uniqueCharCount = 0;

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

TC: O(N)
SC: O(128)

# Simple Solution:

```Java
class Solution 
{
    public int lengthOfLongestSubstringKDistinct(String s, int k) 
    {
        int[] freq = new int[128];
        int left = 0, right = 0;
        int ans = 0, n = s.length();
        int uniqueCharCount = 0;

        while (right < n)
        {
            // right++: Expanding the Window: ALWAYS
            // Post increment freq = 0, Means CURRENT Freq is 0 - Unique
            if (freq[s.charAt(right)]== 0)
            {
                uniqueCharCount++;
                right++;
                freq[s.charAt(right)]++;

                while (uniqueCharCount > k)
                {
                    // left++: Shrinking the Window
                    // Shrink the Window only when there is repeating char and till freq == 0
                    // freq == 0, No More Reepating char
                    if (freq[s.charAt(left)] == 0)
                    {
                        uniqueCharCount--;
                        freq[s.charAt(left)]--;
                        left++;
                    }
                }
            }
            ans = Math.max(ans, right-left);
        }

        return ans;
    }
}
```