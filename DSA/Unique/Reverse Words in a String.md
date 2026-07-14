# [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

Given an input string `s`, reverse the order of the **words**.

A **word** is defined as a sequence of non-space characters. The **words** in `s` will be separated by at least one space.

Return _a string of the words in reverse order concatenated by a single space._

**Note** that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**

**Input:** s = "the sky is blue"
**Output:** "blue is sky the"

**Example 2:**

**Input:** s = "  hello world  "
**Output:** "world hello"
**Explanation:** Your reversed string should not contain leading or trailing spaces.

**Example 3:**

**Input:** s = "a good   example"
**Output:** "example good a"
**Explanation:** You need to reduce multiple spaces between two words to a single space in the reversed string.

**Constraints:**

- `1 <= s.length <= 104`
- `s` contains English letters (upper-case and lower-case), digits, and spaces `' '`.
- There is **at least one** word in `s`.

**Follow-up:** If the string data type is mutable in your language, can you solve it **in-place** with `O(1)` extra space?
# Solution 1 : Split words then Join words in reverse order
## CODE (Mine)
```cpp
class Solution {
public:
    string reverseWords(string s) {
        vector<string> words;
        string word;
        stringstream ss(s);
        //while (std::getline(ss, word, ' ')) {
        //    if (!word.empty())
        //    words.push_back(word);
        //}
        while (ss >> word) {
            words.push_back(word);
        }
        string result_acc = std::accumulate(
            std::next(words.rbegin()), words.rend(), words[words.size()-1],
            [](const std::string& a, const std::string& b) { return a + " " + b; }
        );
        return result_acc;
    }
};
```

**Complexity**
- Time: `O(N)`, where `N <= 10^4` is length of string `s`.
- Space: `O(N)`

# Solution 2 : Reverse whole string then reverse word by word 

- Reverse the whole string.
- Then we iterate characters in string `s`, fill found word into `s` then reverse it, word by word.
    - Let `l` keep the starting index of the current word, `r` keep the next index for filling the current word.
    - Reverse the current word, which is `s[l..r-1]`
    - Set space character for character `s[r]` if `r < s.size()`
- Finally, resize the string `s` to remove redundant chars.

![[Pasted image 20260714142914.png]]
## CODE
```cpp
class Solution {
public:
    string reverseWords(string s) {
        reverse(s.begin(), s.end());
        int i = 0, k = 0, n = s.size();
        while (i < n) {
            // Find the starting pos of the next word
            while (i < n && s[i] == ' ') i++;

            if (i != n && k > 0) { // Still have the next word, add " " here
                s[k++] = ' ';
            }

            int start_index = k;
            // Find the ending pos of that word
            while (i < n && s[i] != ' ') s[k++] = s[i++];

            // Reverse that word
            reverse(s.begin() + start_index, s.begin() + k);
        }
        s.resize(k);
        return s;
    }
};
```
**Complexity**

- Time: `O(N)`, where `N <= 10^4` is length of string `s`.
- Space: `O(1)`