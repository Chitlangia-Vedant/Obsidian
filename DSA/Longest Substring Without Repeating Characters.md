# [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

Given a string `s`, find the length of the **longest** **substring** without duplicate characters.

**Example 1:**

**Input:** s = "abcabcbb"
**Output:** 3
**Explanation:** The answer is "abc", with the length of 3. Note that `"bca"` and `"cab"` are also correct answers.

**Example 2:**

**Input:** s = "bbbbb"
**Output:** 1
**Explanation:** The answer is "b", with the length of 1.

**Example 3:**

**Input:** s = "pwwkew"
**Output:** 3
**Explanation:** The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

**Constraints:**

- `0 <= s.length <= 5 * 104`
- `s` consists of English letters, digits, symbols and spaces.

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        
    }
};
```

# Understanding:

```
abcabcbb

abc
OP: 3
```

```
bbbbb

OP: 1
```

```
pwwkew

pw: 2
wke: 3

OP: 3
```
# Solution:

Translation:

Without Repeating Characters ------> Unique Characters: Set/Map

Longest Substring "Without Repeating Characters"
- Maximum Size of Unique Characters Window

"Maximum Size of Unique Characters Window"

## Approach:

Sliding Window + Set

(1) If Unqiue Characters (Not Present in Set)
Increase Window Size: j++

(2) If Repeating Characters  (Present in Set)
Decrease Window Size: i++

Maintains ans = max(j-i+1)

## DRY RUN:

```
"abcabcbb"

i = 0, j = 0

i = 0, j = 1
s[i] = a, s[j] = b

Unique Characters: Increase Window: j++


i = 0, j = 2
s[i] = a, s[j] = c


Unique Characters: Increase Window: j++

ans = max(j-i+1) = 3


i = 0, j = 3
s[i] = a, s[j] = a

SAME: Repeating Characters: Decrease Window Size: i++
```
# CODE:

```cpp
class Solution {
    public int lengthOfLongestSubstring(String s) 
    {
        int i=0, j=0, max =0;
        HashSet<Character> set = new HashSet<Character>();
        
        while (j < s.length())
        {
            // Not Contained in Set, Increase Window
            if (!set.contains(s.charAt(j)))
            {
                set.add(s.charAt(j));
                j++;
                max  = Math.max(max,set.size());
            }
            
            else
            {
                // Already Contained in Set, Decrease Window
                set.remove(s.charAt(i));
                i++;
            }
        }
            return max;   
    }
}
```

TC: O(N)
SC: O(N)
