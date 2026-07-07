# [126. Word Ladder II](https://leetcode.com/problems/word-ladder-ii/)

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _all the **shortest transformation sequences** from_ `beginWord` _to_ `endWord`_, or an empty list if no such sequence exists. Each sequence should be returned as a list of the words_ `[beginWord, s1, s2, ..., sk]`.

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
**Output:** [["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]
**Explanation:** There are 2 shortest transformation sequences:
"hit" -> "hot" -> "dot" -> "dog" -> "cog"
"hit" -> "hot" -> "lot" -> "log" -> "cog"

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
**Output:** []
**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

**Constraints:**

- `1 <= beginWord.length <= 5`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 500`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.
- The **sum** of all shortest transformation sequences does not exceed `105`.

```cpp
class Solution {
public:
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        
    }
};
```

# Solution 1
## CODE (Mine)

```cpp
class Solution {
public:
    vector<vector<string>> res;
    vector<string> path;
    vector<vector<string>> findLadders(string beginWord, string endWord, vector<string>& wordList) {
        if(!ranges::contains(wordList,endWord)){
            return {};
        }
        unordered_map<string,pair<vector<string>,int>> mp;
        queue<string> q;
        q.push(endWord);
        mp[endWord]={};
        while(!q.empty()){
            for(auto &i:wordList){
                int count=0;
                for(int j=0;j<beginWord.length()&&count<=1;j++){
                    if(q.front()[j]!=i[j]){
                        count++;
                    }
                }
                if(count==1){
                    if(!mp.contains(i)){
                        mp[i].first.push_back(q.front());
                        mp[i].second=mp[q.front()].second+1; 
                        q.push(i);
                    }else if(mp[i].second-1==mp[q.front()].second){
                        mp[i].first.push_back(q.front());
                    }
                }   
            }
            q.pop();
        }
        if(!mp.contains(beginWord)){
        for(auto &i:wordList){
            int count=0;
            for(int j=0;j<beginWord.length()&&count<=1;j++){
                if(beginWord[j]!=i[j]){
                    count++;
                }
            }
            if(count==1 && mp.contains(i)){
                if(!mp.contains(beginWord)){
                    mp[beginWord].first.push_back(i);
                    mp[beginWord].second=mp[i].second+1;
                    
                }else if(mp[beginWord].second-1==mp[i].second){
                    mp[beginWord].first.push_back(i);
                }
                    
            }
        }
        }
        if(mp.contains(beginWord))
            dfs(beginWord, endWord, mp);
        return res;
    }
    void dfs(string word, string endWord,unordered_map<string, pair<vector<string>, int>>& mp)
    {
        path.push_back(word);

        if(word == endWord){
            res.push_back(path);
        }
        else{
            for(auto &next : mp[word].first){
                dfs(next, endWord, mp);
            }
        }

        path.pop_back();
    }
};
```