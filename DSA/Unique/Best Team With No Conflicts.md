# [1626. Best Team With No Conflicts](https://leetcode.com/problems/best-team-with-no-conflicts/)

You are the manager of a basketball team. For the upcoming tournament, you want to choose the team with the highest overall score. The score of the team is the **sum** of scores of all the players in the team.

However, the basketball team is not allowed to have **conflicts**. A **conflict** exists if a younger player has a **strictly higher** score than an older player. A conflict does **not** occur between players of the same age.

Given two lists, `scores` and `ages`, where each `scores[i]` and `ages[i]` represents the score and age of the `ith` player, respectively, return _the highest overall score of all possible basketball teams_.

**Example 1:**

**Input:** scores = [1,3,5,10,15], ages = [1,2,3,4,5]
**Output:** 34
**Explanation:** You can choose all the players.

**Example 2:**

**Input:** scores = [4,5,6,5], ages = [2,1,2,1]
**Output:** 16
**Explanation:** It is best to choose the last 3 players. Notice that you are allowed to choose multiple people of the same age.

**Example 3:**

**Input:** scores = [1,2,3,5], ages = [8,9,10,1]
**Output:** 6
**Explanation:** It is best to choose the first 3 players. 

**Constraints:**

- `1 <= scores.length, ages.length <= 1000`
- `scores.length == ages.length`
- `1 <= scores[i] <= 106`
- `1 <= ages[i] <= 1000`

```cpp
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        
    }
};
```
# Solution 1

1. Create a pair array that contains the ages and score of players.
2. Store each player's score and age as a pair in the above array.
3. Sort the pair array in increasing order of age.(If age is same then sort in increasing order of score)
4. Initialize an map `mp` (`mp[i]` will contain the maximum score including the player `i` and older players)  
5. Initialize a variable `mx` to store the maximum overall score.
6. Loop through the pair array from index `i = players.size()-1` to `i = 0`.
7. For each `i`, loop through j from 1 to `players.size()-1`. (`i+j` goes from `i+1` to end of array )
8. If the score of player `i+j` is greater than or equal to the score of player `i`, update `mp[i]` to be the maximum of `mp[i]` and `mp[i+j] + score[i]`.(`score[i]=players[i].second`)
9. Set `mx` to be the maximum of `mx` and `mp[i]`.
10. Return `mx` as the result.
## CODE (Mine)

```cpp
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        vector<pair<int,int>> players;
        for(int i=0;i<scores.size();i++){
            players.push_back({ages[i],scores[i]});
        }
        sort(players.begin(),players.end());
        //for(auto &i:players){
        //    cout<<i.first<<":"<<i.second<<"\n";
        //}
        int mx=0;
        unordered_map<int,int> mp;
        for(int i=players.size()-1;i>=0;i--){
            int j,m=0;
            for(j=1;i+j<players.size();j++){
                if(players[i+j].second>=players[i].second){
                    m=max(m,mp[i+j]);
                }
            }
            mp[i]=players[i].second+m;
            mx=max(mx,mp[i]);
            //cout<<k<<" "<<mp[i]<<"\n";
        }
        return mx;
    }
};
```

## Time Complexity and Space Complexity:
- Time complexity: **O(n^2)**
- Space complexity: **O(n)** // using extra space of size n

## Extra
We can also iterate from `i = 0` to `i = players.size() - 1` and  `j` from `0` to `i - 1`.
`mp[i]` will contain the maximum score including player `i` and younger
# Solution 2

1. Create a tuple array `items` that contains the score and age of each player.
2. Store each player's score and age as a tuple `(score, age)` in the above array.
3. Sort the tuple array in increasing order of score. (If scores are the same, sort in increasing order of age.)
4. Initialize a map `res`, where the key represents an age and the value represents the maximum team score achievable with players of age up to that key. Insert the initial pair `(0, 0)` into the map.
5. Traverse each player in the sorted tuple array.
6. For the current player, use `upper_bound(age)` to find the first map entry with an age greater than the current player's age. Move one step back to obtain the maximum team score among players whose age is less than or equal to the current player's age.
7. Compute the new team score by adding the current player's score to the value obtained in the previous step.
8. Insert the pair `(current age, new team score)` into the map. If an entry for the same age already exists with a smaller score, update it with the larger score.
9. Remove all subsequent map entries whose stored team score is less than or equal to the current team score, as they can never produce a better answer in future iterations.
10. After processing all players, return the value of the last element in the map, which represents the maximum possible team score.
## CODE

```cpp
class Solution {
public:
    int bestTeamScore(vector<int>& scores, vector<int>& ages) {
        vector<tuple<int,int>> items;
        for (int i = 0; i < size(scores); ++i) items.push_back({scores[i], ages[i]});
        sort(begin(items), end(items));

        map<int,int> res; res[0] = 0;
        for(auto [sc, ag] : items) {
            auto it0 = res.upper_bound(ag);
            int sc2 = sc + (--it0)->second;
            auto it = res.insert(it0, {ag, sc2});
            if (it->second < sc2) it->second = sc2;
            ++it;
            while (it != res.end() && it->second <= sc2) {
                auto it2 = it++;
                res.erase(it2);
            }
        }
        return res.rbegin()->second;
    }
};
```

## Complexity

- **Time complexity:**  
    The time complexity of this solution is **O(N log N)**, where N is the number of players. This is because sorting the players based on their scores and ages takes O(N log N) time, and each player is processed once, which takes O(log N) time to update the map.
    
- **Space complexity:**  
    The space complexity of this solution is **O(N)**, because it stores the maximum total score for each age in the map, so the number of entries in the map is proportional to N.