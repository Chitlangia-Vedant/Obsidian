[763. Partition Labels](https://leetcode.com/problems/partition-labels/)

You are given a string `s`. We want to partition the string into as many parts as possible so that each letter appears in at most one part. For example, the string `"ababcc"` can be partitioned into `["abab", "cc"]`, but partitions such as `["aba", "bcc"]` or `["ab", "ab", "cc"]` are invalid.

Note that the partition is done so that after concatenating all the parts in order, the resultant string should be `s`.

Return _a list of integers representing the size of these parts_.

**Example 1:**

**Input:** s = "ababcbacadefegdehijhklij"
**Output:** [9,7,8]
**Explanation:**
The partition is "ababcbaca", "defegde", "hijhklij".
This is a partition so that each letter appears in at most one part.
A partition like "ababcbacadefegde", "hijhklij" is incorrect, because it splits s into less parts.

**Example 2:**

**Input:** s = "eccbbbbdec"
**Output:** [10]

**Constraints:**

- `1 <= s.length <= 500`
- `s` consists of lowercase English letters.
# Intuition

For every character, all of its occurrences must be inside the same partition.  
Your code builds partitions as we scan the string. Initially, every new character starts a new partition of size `1`.  
When we see a character again, we know its first and current occurrence must belong to the same partition. So we merge the most recent partitions until they cover the entire range between those two occurrences.  
A `stack` is useful because we only ever need to merge partitions from the right side.  
For example:  
`ababcbaca`  
When the second `a` appears, the partitions between the first `a` and this `a` must all be merged into one partition.

# Algorithm

1. `mp` stores the first and latest position of every character:
```cpp
unordered_map<char,pair<int,int>> mp;
```

2. `res` is a stack containing the sizes of the current partitions:
```cpp
stack<int> res;
```

3. For every character:
	- If it is new, create a partition of size `1`.
	- If it has appeared before, update its latest position.
4. For a repeated character, calculate how many characters are between its first and current occurrence:
```cpp
mp[s[i]].second - mp[s[i]].first
```

5. Pop partitions from the stack and add their sizes until their total covers this distance:
```cpp
while(!res.empty() && sum < (mp[s[i]].second - mp[s[i]].first)){
    sum += res.top();
    res.pop();
}
```

6. Add `1` for the current character and push the merged partition:
```cpp
res.push(++sum);
```

7. At the end, the stack contains the partition sizes in reverse order, so you pop them into `ans` from right to left.
# CODE (Mine)

```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        unordered_map<char,pair<int,int>> mp;
        stack<int> res;
        for(int i=0;i<s.length();i++){
            if(mp.contains(s[i])) {
                mp[s[i]].second=i;
                int sum=0;
                while(!res.empty()&&sum<(mp[s[i]].second-mp[s[i]].first)){
                    sum+=res.top();
                    res.pop();
                }
                res.push(++sum);
            }
            else {
                res.push(1);
                mp[s[i]]={i,i};
            }
        }
        vector<int> ans(res.size());
        int i=res.size()-1;
        while(!res.empty()){
            ans[i--]=res.top();
            res.pop();
        }
        
        return ans;
    }
};
```
# Dry Run

Take:

```text
s = "ababcbacadefegdehijhklij"
```

We eventually get:

```text
[9,7,8]
```

Start:

```text
res = []
```

`a` → new:

```text
res = [1]
```

`b` → new:

```text
res = [1,1]
```

`a` → repeated. First `a=0`, current `a=2`.  
Distance = `2`.  
Pop the latest partition:

```text
sum = 1
```

Still need more, so pop again:

```text
sum = 2
```

Add current character:

```text
sum = 3
res = [3]
```

`b` → repeated. First `b=1`, current `b=3`.  
Distance = `2`.  
Pop `3`. Since `3 >= 2`, the entire existing partition already covers the range.  
Add current character:

```text
sum = 4
res = [4]
```

The same process continues. When we reach the final `a` at index `8`, the partition has grown to:

```text
"ababcbaca"
size = 9
res = [9]
```

Then `d,e,f,g` form another partition. Repeated `e` forces the necessary pieces to merge:

```text
"defegde"
size = 7
res = [9,7]
```

Finally:

```text
"hijhklij"
size = 8
res = [9,7,8]
```

The stack is then reversed into the answer:

```text
ans = [9,7,8]
```

# Important Code Sections

### New character

```cpp
else {
    res.push(1);
    mp[s[i]]={i,i};
}
```

A character that hasn't appeared before starts a new partition.

### Repeated character

```cpp
mp[s[i]].second=i;
```

Update its latest occurrence.

Then:

```cpp
int sum=0;
while(!res.empty() && sum < (mp[s[i]].second-mp[s[i]].first)){
    sum += res.top();
    res.pop();
}
```

Merge recent partitions until they cover the distance between the first and current occurrence.

Finally:

```cpp
res.push(++sum);
```

Include the current character and store the merged partition size.

### Reverse the stack

```cpp
vector<int> ans(res.size());
int i=res.size()-1;

while(!res.empty()){
    ans[i--]=res.top();
    res.pop();
}
```

Because a stack gives elements from right to left, you place them into `ans` backwards.

# Time & Space Complexity

**Time: `O(n)`**

Although there is a `while` loop inside the `for` loop, every partition that gets popped is removed permanently. So across the entire algorithm, each partition is pushed and popped at most once.

**Space: `O(n)`**

The stack and map use at most `O(n)` space.
# Key Idea

`Create partitions greedily → when a repeated character crosses partitions, merge those partitions → continue until every character belongs to only one partition.`  
