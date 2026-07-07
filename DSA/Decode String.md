# [394. Decode String](https://leetcode.com/problems/decode-string/)

Given an encoded string, return its decoded string.

The encoding rule is: `k[encoded_string]`, where the `encoded_string` inside the square brackets is being repeated exactly `k` times. Note that `k` is guaranteed to be a positive integer.

You may assume that the input string is always valid; there are no extra white spaces, square brackets are well-formed, etc. Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, `k`. For example, there will not be input like `3a` or `2[4]`.

The test cases are generated so that the length of the output will never exceed `105`.

**Example 1:**

**Input:** s = "3[a]2[bc]"
**Output:** "aaabcbc"

**Example 2:**

**Input:** s = "3[a2[c]]"
**Output:** "accaccacc"

**Example 3:**

**Input:** s = "2[abc]3[cd]ef"
**Output:** "abcabccdcdcdef"

**Constraints:**

- `1 <= s.length <= 30`
- `s` consists of lowercase English letters, digits, and square brackets `'[]'`.
- `s` is guaranteed to be **a valid** input.
- All the integers in `s` are in the range `[1, 300]`.

```cpp
class Solution {
public:
    string decodeString(string s) {
        
    }
};
```

# Solution 1: Recursion

## CODE (Mine)

```cpp
class Solution {
public:
    string decodeString(string s) {
        string res="";
        int repeat=0;
        int inbracket=0;
        string text="";
        for(auto &c:s){
            //cout<<c<<" "<<text<<"\n";
            if(isdigit(c)&&inbracket==0){
                repeat=(repeat*10)+(c-'0');
                continue;
            }
            if(c=='['&&(inbracket++)==0){
                res+=text;
                text="";
                continue;
            }
            if(c==']'&&(inbracket--)==1){
                text=decodeString(text);
                for(int i=0;i<repeat;i++){
                    res+=text;
                }
                repeat=0;
                text="";
                inbracket=false;
                continue;
            }
            text+=c;
        }
        res+=text;
        return res;
    }
    
};
```