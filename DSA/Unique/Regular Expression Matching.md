# [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

- `'.'` Matches any single character.​​​​
- `'*'` Matches zero or more of the preceding element.

Return a boolean indicating whether the matching covers the entire input string (not partial).

**Example 1:**

**Input:** s = "aa", p = "a"
**Output:** false
**Explanation:** "a" does not match the entire string "aa".

**Example 2:**

**Input:** s = "aa", p = "a*"
**Output:** true
**Explanation:** '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

**Example 3:**

**Input:** s = "ab", p = ".*"
**Output:** true
**Explanation:** ".*" means "zero or more (*) of any character (.)".

**Constraints:**

- `1 <= s.length <= 20`
- `1 <= p.length <= 20`
- `s` contains only lowercase English letters.
- `p` contains only lowercase English letters, `'.'`, and `'*'`.
- It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

```cpp
class Solution {
public:
    bool isMatch(string s, string p) {
        
    }
};
```
# Solution 1

## CODE(Mine)
```cpp
class Solution {
public:
    vector<vector<int>> dp;
    string s;
    string p;
    bool isMatch(string s, string p) {
        dp.assign(s.length()+1,vector<int>(p.length()+1,-1));
        this->s=s;
        this->p=p;
        return match(0,0)==1;
    }
    int match(int i,int j){
        if(dp[i][j]!=-1) return dp[i][j];
        if (j == p.size()){
            dp[i][j]=i == s.size()?1:0;
            return dp[i][j];
        }
        dp[i][j]=0;
        if(i==s.size()){
            if(p[j+1]=='*')
                dp[i][j]=max(dp[i][j],match(i,j+2));
            return dp[i][j];
        }
        
        if(s[i]==p[j]){
            if(p[j+1]=='*') {
                dp[i][j]=max(dp[i][j],match(i+1,j));
                dp[i][j]=max(dp[i][j],match(i,j+2));
                dp[i][j]=max(dp[i][j],match(i+1,j+2));
            }else{
                dp[i][j]=max(dp[i][j],match(i+1,j+1));
            }
        }
        if(p[j]=='.'){
            if(p[j+1]=='*'){
                dp[i][j]=max(dp[i][j],match(i+1,j));
                dp[i][j]=max(dp[i][j],match(i,j+2));
                dp[i][j]=max(dp[i][j],match(i+1,j+2));
            }else{
                dp[i][j]=max(dp[i][j],match(i+1,j+1));
            }
        }
        if(s[i]!=p[j]&&p[j+1]=='*'){
            dp[i][j]=max(dp[i][j],match(i,j+2));
        }
        return dp[i][j];
    }
};
```