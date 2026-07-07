# [387. First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/)

Given a string `s`, find the **first** non-repeating character in it and return its index. If it **does not** exist, return `-1`.

**Example 1:**

**Input:** s = "leetcode"

**Output:** 0

**Explanation:**

The character `'l'` at index 0 is the first character that does not occur at any other index.

**Example 2:**

**Input:** s = "loveleetcode"

**Output:** 2

**Example 3:**

**Input:** s = "aabb"

**Output:** -1

**Constraints:**

- `1 <= s.length <= 105`
- `s` consists of only lowercase English letters.

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        
    }
};
```

# Understanding:


leetcode
Non Repeating Characters: l, t, c, o, d
First Non Repeating Character: l
Index of l: 0 - OP

loveleetcode
Non Repeating Characters: v, t, c, d
First Non Repeating Character: v
Index of v: 2 - OP
# Solution:

## (1) Brute Force Solution:
Two Nested Loops

Check for first character which is Non-Repeated, return from there

TC: O(N^2)
SC: O(1)
## (2) Map

Key: char
Value: freq

```
if (freq==1)
	return char;
```

Order is Important:
- HashMap: Does Not Maintain Order
- LinkedHashMap: Maintains Order
- TreeMap: Stores in the Sorted Fashion

Right Choice:
LinkedHashMap

```
arr = [3 4 2 1 5]

HashMap: 
{3:1, 4:1, 2:1, 1:1, 5:1} OR {5:1, 1:1, 2:1, 4:1, 3:1}

LinkedHashMap: {3:1, 4:1, 2:1, 1:1, 5:1} - ALWAYS

TreeMap: {1:1, 2:1, 3:1, 4:1, 5:1}
```

Java: LinkedHashMap
C++: UnOrdered Map

### CODE
```java
class Solution {
    public int firstUniqChar(String s) {
        HashMap<Character, Integer> count = new HashMap<Character, Integer>();
        int n = s.length();
        // build hash map : character and how often it appears
        for (int i = 0; i < n; i++) {
            char c = s.charAt(i);
            count.put(c, count.getOrDefault(c, 0) + 1);
        }
        
        // find the index
        for (int i = 0; i < n; i++) {
            if (count.get(s.charAt(i)) == 1) 
                return i;
        }
        return -1;
    }
}
```
## (3) Frequency Array

freq[26];

Check for freq==1

Order is Important, Need to check for Order

Compare with the String to Maintain Order

CODE:

```cpp
class Solution {
public:
    int firstUniqChar(string s) {
        int freq[26] ;
        int i = 0;

        for (i=0; i<s.length(); i++)
        {
            freq[s[i] - 'a']++;
        }

        for (i=0; i<s.length(); i++)
        {
            if (freq[s[i] - 'a'] == 1)
                return i;
        }

        return -1;
    }
};
```

TC: O(N) + O(N)
SC: O(26)