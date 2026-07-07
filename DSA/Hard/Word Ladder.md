# [127. Word Ladder](https://leetcode.com/problems/word-ladder/)

A **transformation sequence** from word `beginWord` to word `endWord` using a dictionary `wordList` is a sequence of words `beginWord -> s1 -> s2 -> ... -> sk` such that:

- Every adjacent pair of words differs by a single letter.
- Every `si` for `1 <= i <= k` is in `wordList`. Note that `beginWord` does not need to be in `wordList`.
- `sk == endWord`

Given two words, `beginWord` and `endWord`, and a dictionary `wordList`, return _the **number of words** in the **shortest transformation sequence** from_ `beginWord` _to_ `endWord`_, or_ `0` _if no such sequence exists._

**Example 1:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log","cog"]
**Output:** 5
**Explanation:** One shortest transformation sequence is "hit" -> "hot" -> "dot" -> "dog" -> cog", which is 5 words long.

**Example 2:**

**Input:** beginWord = "hit", endWord = "cog", wordList = ["hot","dot","dog","lot","log"]
**Output:** 0
**Explanation:** The endWord "cog" is not in wordList, therefore there is no valid transformation sequence.

**Constraints:**

- `1 <= beginWord.length <= 10`
- `endWord.length == beginWord.length`
- `1 <= wordList.length <= 5000`
- `wordList[i].length == beginWord.length`
- `beginWord`, `endWord`, and `wordList[i]` consist of lowercase English letters.
- `beginWord != endWord`
- All the words in `wordList` are **unique**.

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        
    }
};
```

# Solution 1

## CODE (Mine)
```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        if(!ranges::contains(wordList,endWord)){
            return 0;
        }
        unordered_map<string,int> mp;
        queue<string> q;
        q.push(endWord);
        mp[endWord]=0;
        while(!q.empty()){
            //cout<<q.front()<<"\n";
            for(auto &i:wordList){
                int count=0;
                for(int j=0;j<beginWord.length()&&count<=1;j++){
                    if(q.front()[j]!=i[j]){
                        count++;
                    }
                }
                if(count==1){
                    if(!mp.contains(i)){
                        mp[i]=mp[q.front()]+1;
                        q.push(i);
                    }
                }
            }
            q.pop();
        }
        int res=INT_MAX;
        for(auto &i:wordList){
            int count=0;
            for(int j=0;j<beginWord.length()&&count<=1;j++){
                if(beginWord[j]!=i[j]){
                    count++;
                }
            }
            if(count==1){
                if(mp.contains(i))
                    res=min(res,mp[i]+1);
            }
        }
        return res==INT_MAX?0:res+1;
    }
};
```

# Solution 2

Let's take an example:-

```java
start = be
end = ko
words = ["ce", "mo", "ko", "me", "co"]
```

So, using this word list we have 4 different path's that we can take to get from our start to end word.

- So, we can go from `"be" -- "ce" -- "co" -- "ko"`

There are other's path as well to go from start word to end word, however we only consider the shortest path, the one that has least amount of word in the sequence and in that case that would be:

![image](https://assets.leetcode.com/users/images/7d41268c-da5f-40ce-a11b-e16b7e27e875_1644630682.2860324.png)

So, in a typicall `Breadth-First-Search we utilize the queue` and it's going to store each string that in our sequence & then we also going to have integer value called changes which will be eventually return from our function, which will keep track how many changes do we have in the sequence.

So, we intialize our **queue** that have starting word inside of it i.e. **"be",** then our **changes** variable is going to **start at 1**, this is because at minimum we going to have starting word in our minimum. And finally we have a **set** which will **keep track's of node** that have been **visited**, in this case we just **keeping track of string** that we have already **added inside our queue**.

```cpp
queue = ["be"  ]
changes = 1
set = ["be"  ]
```

So, to start of our bfs, we take "be" off from our queue & we can only change one character at a time. So, first we gonna check by changing the character **b**, if we can form another word inside our word list.

- So, we try **ae**, which is not in our word list. Then, **be** is already in our set, so we can't use that. Now we try **ce** and we have that word inside our word list. With that means add **ce** in our queue.
    
- Then we check **de, fe** and so on............ until we get to **me** which is inside our word list, so we add **me** inside our word list as well.
    
- So, all that way we check all the way down to **ze** and there is no other words that we add by changing **b** to another character.
    

![image](https://assets.leetcode.com/users/images/6d8dd6e4-aaee-4d89-a97c-2b90cba4ce9d_1644631773.493334.png)

So, now we need to check first index. So, by changing the character **e** from **be** to something else if we form an another word.

- So, we gonna check **ba, bb** all the way down to **bz**. However none of the word inside our word list.
- Adittionaly, thw word **ce & me** are going to get added inside our set. To show, that we have already visited those words.

![image](https://assets.leetcode.com/users/images/0b6666c7-1a53-43de-aecb-9dc831096fd8_1644633217.5717092.png)

And then we gonna perform same logic as before. We **poll from our queue** we take **ce** off from our queue. And we check if by changing any character we can form another word.

- So, we gonna see if we change first character **c** to another character to form a word. So, we gonna try **ae** **, be, ce** which is already in our set doesn't count **de, ee** all the ay down to **ze** and none of the word included inside our word list.

![image](https://assets.leetcode.com/users/images/e580b3e4-5ebd-46e7-b546-f63e46749ed1_1644633248.873971.png)

Now we gonna see if we change **e** from **ce** to another character.

- So, we gonna try **ca, cb** and so on..... eventually we get to the word **co** which is in our word list. So what that means we gonna add **co** to our queue & our set.

![image](https://assets.leetcode.com/users/images/10697e6c-f2a9-4631-bb8f-271a0d4ca139_1644633282.432196.png)

Then we gonna **pull from our queue** again, in this case it would be **me**

- Now from **me** we can go to **mo** by changing **e** from **me** to **o** from **mo**. So, we gonna add **mo** to our queue and our set.

![image](https://assets.leetcode.com/users/images/ad0a8570-27f4-4e75-8ac9-dd6d5f16c8ba_1644633419.043082.png)

```csharp
Another, thing i forgot to mention is our changes variable is being updated on every iteration.
So, we have gone from be -> ce -> co which is a total of 3 changes.
```

So, we gonna pull from our queue again **co**, now we can go from **co** to **ko** by changing **c** to **k**. So, what that mean's we need to add **ko** inside our queue and our set. And we increase our **changes** variable by **1**

![image](https://assets.leetcode.com/users/images/930fbdd7-774e-4384-93da-be75ee4b31be_1644633808.140633.png)

Now we pull from our queue again, which would be **mo**. We can get from **mo -> ko** by changing **m** to **k**. However, **ko** is already in our set. So, that mean's we gonna pull from our queue again which in this case would be **ko**.

```cpp
queue = [  ]
changes = 4
set = ["be", "ce", "me", "co", "mo", "ko"  ]
```

And that is our **end word.** Once, we find our **end word** we poll from our queue and that is equal end word then we know we have found our shortest path sequence, so we just **return 4** from this function.

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> st(wordList.begin(), wordList.end());
        if (!st.count(endWord))
            return 0;
        queue<string> q;
        q.push(beginWord);
        unordered_set<string> visited;
        visited.insert(beginWord);
        int changes = 1;
        while (!q.empty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                string word = q.front();
                q.pop();
                if (word == endWord)
                    return changes;
                for (int j = 0; j < word.length(); j++) {
                    string temp = word;
                    for (char c = 'a'; c <= 'z'; c++) {
                        temp[j] = c;
                        if (st.count(temp) && !visited.count(temp)) {
                            q.push(temp);
                            visited.insert(temp);
                        }
                    }
                }
            }
            changes++;
        }
        return 0;
    }
};
```

ANALYSIS :-

- **Time Complexity :-** BigO(M^2 * N), where M is size of dequeued word & N is size of our word list
    
- **Space Complexity :-** BigO(M * N) where M is no. of character that we had in our string & N is the size of our wordList.

# Solution 3: Two Ended BFS

The above code starts from a single end `beginWord`. We may also start from the `endWord` simultaneously. Once we meet the same word, we are done.

```cpp
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> dict(wordList.begin(), wordList.end()), head, tail, *phead, *ptail;
        if (dict.find(endWord) == dict.end()) {
            return 0;
        }
        head.insert(beginWord);
        tail.insert(endWord);
        int ladder = 2;
        while (!head.empty() && !tail.empty()) {
            if (head.size() < tail.size()) {
                phead = &head;
                ptail = &tail;
            } else {
                phead = &tail;
                ptail = &head;
            }
            unordered_set<string> temp;
            for (auto it = phead -> begin(); it != phead -> end(); it++) {    
                string word = *it;
                for (int i = 0; i < word.size(); i++) {
                    char t = word[i];
                    for (int j = 0; j < 26; j++) {
                        word[i] = 'a' + j;
                        if (ptail -> find(word) != ptail -> end()) {
                            return ladder;
                        }
                        if (dict.find(word) != dict.end()) {
                            temp.insert(word);
                            dict.erase(word);
                        }
                    }
                    word[i] = t;
                }
            }
            ladder++;
            phead -> swap(temp);
        }
        return 0;
    }
};
```