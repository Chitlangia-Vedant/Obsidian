# [1366. Rank Teams by Votes](https://leetcode.com/problems/rank-teams-by-votes/)

In a special ranking system, each voter gives a rank from highest to lowest to all teams participating in the competition.

The ordering of teams is decided by who received the most position-one votes. If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter.

You are given an array of strings `votes` which is the votes of all voters in the ranking systems. Sort all teams according to the ranking system described above.

Return _a string of all teams **sorted** by the ranking system_.

**Example 1:**

**Input:** votes = ["ABC","ACB","ABC","ACB","ACB"]
**Output:** "ACB"
**Explanation:** 
Team A was ranked first place by 5 voters. No other team was voted as first place, so team A is the first team.
Team B was ranked second by 2 voters and ranked third by 3 voters.
Team C was ranked second by 3 voters and ranked third by 2 voters.
As most of the voters ranked C second, team C is the second team, and team B is the third.

**Example 2:**

**Input:** votes = ["WXYZ","XYZW"]
**Output:** "XWYZ"
**Explanation:**
X is the winner due to the tie-breaking rule. X has the same votes as W for the first position, but X has one vote in the second position, while W does not have any votes in the second position. 

**Example 3:**

**Input:** votes = ["ZMNAGUEDSJYLBOPHRQICWFXTVK"]
**Output:** "ZMNAGUEDSJYLBOPHRQICWFXTVK"
**Explanation:** Only one voter, so their votes are used for the ranking.

**Constraints:**

- `1 <= votes.length <= 1000`
- `1 <= votes[i].length <= 26`
- `votes[i].length == votes[j].length` for `0 <= i, j < votes.length`.
- `votes[i][j]` is an English **uppercase** letter.
- All characters of `votes[i]` are unique.
- All the characters that occur in `votes[0]` **also occur** in `votes[j]` where `1 <= j < votes.length`.

```cpp
class Solution {
public:
    string rankTeams(vector<string>& votes) {
        
    }
};
```
# Solution 1: 

1. Determine the number of teams `len` from the length of the first vote.
2. Create a 2D vector `vec` of size `len × (len + 1)`, initialized with `0`. Each row represents a team, the first `len` columns store the number of votes received at each rank, and the last column stores the team's character (as its negative ASCII value).
3. Create an unordered map `mp` to map each team character to its corresponding row index in `vec`.
4. Traverse the teams in the first vote. For each team, store its row index in `mp` and save its negative ASCII value in the last column of its row. This is used as a tie-breaker to rank teams alphabetically.
5. Traverse every vote in the input.
6. For each position `i` in a vote, use `mp` to find the corresponding row for that team and increment the count in column `i`, indicating that the team received one more vote for rank `i`.
7. Sort the rows of `vec` in descending lexicographical order using `std::greater<>()`. This prioritizes teams with more first-place votes, then second-place votes, and so on. If all vote counts are equal, the stored negative ASCII value ensures alphabetical ordering.
8. Initialize an empty string `res`.
9. Traverse the sorted rows of `vec`, convert the negative ASCII value stored in the last column back to its corresponding character, and append it to `res`.
10. Return `res` as the final ranking of the teams.

## CODE (Mine)

```cpp
class Solution {
public:
    string rankTeams(vector<string>& votes) {
        int len=votes[0].length();
        vector<vector<int>> vec (len,vector<int>(len+1,0));
        unordered_map<char,int> mp;
        for(int i=0;i<len;i++){
            mp[votes[0][i]]=i;
            vec[i][len]=-votes[0][i];
        }
        for(auto &vote:votes){
            for(int i=0;i<len;i++){
                vec[mp[vote[i]]][i]++;
            }
        }
        sort(vec.begin(),vec.end(),std::greater<>());
        string res="";
        for(auto &i:vec){
            res+=static_cast<char>(-i[len]);
        }
        return res;
    }
};
```

## CODE (Optimized)

```cpp
    string rankTeams(vector<string>& votes) {
        vector<vector<int>> count(26, vector<int>(27));
        for (char& c: votes[0])
            count[c - 'A'][26] = c;

        for (string& vote: votes)
            for (int i = 0; i < vote.length(); ++i)
                --count[vote[i] - 'A'][i];
        sort(count.begin(), count.end());
        string res;
        for (int i = 0; i < votes[0].length(); ++i)
            res += count[i][26];
        return res;
    }
```

# Solution 2: Custom sort

1. We iterate over the string vector `votes`, and find the character frequency at each location.  
    Locations range for all positions till n (where `n = sizeof(votes[0])`).
2. After we get the frequency vector we simply sort it following the condition :  
    `If two or more teams tie in the first position, we consider the second position to resolve the conflict, if they tie again, we continue this process until the ties are resolved. If two or more teams are still tied after considering all positions, we rank them alphabetically based on their team letter.`
3. The code is pretty simple and easy to understand.

- C++ code

```cpp
class Solution {
public:
    string rankTeams(vector<string>& votes) 
    {
        int n = votes[0].size();
        string ans = votes[0];
        
        vector<vector<int>> freq(26, vector<int> (n));
        
        for(auto i : votes)
        {
            for(int j=0;j<n;j++)
            {
                freq[i[j]-'A'][j]++; // Step 1.
            }
        }
        
        sort(ans.begin(), ans.end(), [&](auto& x, auto& y)
             {
                if(freq[x-'A'] == freq[y-'A'])
                    return x<y;
                 
                 return freq[x-'A'] > freq[y-'A'];
             }); // Step 2
        
        return ans;
    }
};
```
