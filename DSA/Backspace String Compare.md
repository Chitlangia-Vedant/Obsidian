# [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)

Given two strings `s` and `t`, return `true` _if they are equal when both are typed into empty text editors_. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

**Example 1:**

**Input:** s = "ab#c", t = "ad#c"
**Output:** true
**Explanation:** Both s and t become "ac".

**Example 2:**

**Input:** s = "ab##", t = "c#d#"
**Output:** true
**Explanation:** Both s and t become "".

**Example 3:**

**Input:** s = "a#c", t = "b"
**Output:** false
**Explanation:** s becomes "c" while t becomes "b".

**Constraints:**

- `1 <= s.length, t.length <= 200`
- `s` and `t` only contain lowercase letters and `'#'` characters.

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

# Solution 1

## CODE(Mine)

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        stack<char> st1;
        stack<char> st2;
        for(auto &c:s){
            if(c=='#'&&!st1.empty()){
                st1.pop();
            }else if(c!='#'){
                st1.push(c);
            }
        }
        for(auto &c:t){
            if(c=='#'&&!st2.empty()){
                st2.pop();
            }else if(c!='#'){
                st2.push(c);
            }
        }
        return st1==st2;
    }
};
```

# Solution 2

We can do a back string compare (just like the title of problem).  
If we do it backward, we meet a char and we can be sure this char won't be deleted.  
If we meet a '#', it tell us we need to skip next lowercase char.

The idea is that, read next letter from end to start.  
If we meet `#`, we increase the number we need to step back, until `back = 0`  
  
## CODE

```cpp
    bool backspaceCompare(string S, string T) {
        int i = S.length() - 1, j = T.length() - 1, back;
        while (true) {
            back = 0;
            while (i >= 0 && (back > 0 || S[i] == '#')) {
                back += S[i] == '#' ? 1 : -1;
                i--;
            }
            back = 0;
            while (j >= 0 && (back > 0 || T[j] == '#')) {
                back += T[j] == '#' ? 1 : -1;
                j--;
            }
            if (i >= 0 && j >= 0 && S[i] == T[j]) {
                i--;
                j--;
            } else {
                break;
            }
        }
        return i == -1 && j == -1;
    }
```