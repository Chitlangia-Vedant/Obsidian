# [30. Substring with Concatenation of All Words](https://leetcode.com/problems/substring-with-concatenation-of-all-words/)

You are given a string `s` and an array of strings `words`. All the strings of `words` are of **the same length**.

A **concatenated string** is a string that exactly contains all the strings of any permutation of `words` concatenated.

- For example, if `words = ["ab","cd","ef"]`, then `"abcdef"`, `"abefcd"`, `"cdabef"`, `"cdefab"`, `"efabcd"`, and `"efcdab"` are all concatenated strings. `"acdbef"` is not a concatenated string because it is not the concatenation of any permutation of `words`.

Return an array of _the starting indices_ of all the concatenated substrings in `s`. You can return the answer in **any order**.

**Example 1:**

**Input:** s = "barfoothefoobarman", words = ["foo","bar"]

**Output:** [0,9]

**Explanation:**

The substring starting at 0 is `"barfoo"`. It is the concatenation of `["bar","foo"]` which is a permutation of `words`.  
The substring starting at 9 is `"foobar"`. It is the concatenation of `["foo","bar"]` which is a permutation of `words`.

**Example 2:**

**Input:** s = "wordgoodgoodgoodbestword", words = ["word","good","best","word"]

**Output:** []

**Explanation:**

There is no concatenated substring.

**Example 3:**

**Input:** s = "barfoofoobarthefoobarman", words = ["bar","foo","the"]

**Output:** [6,9,12]

**Explanation:**

The substring starting at 6 is `"foobarthe"`. It is the concatenation of `["foo","bar","the"]`.  
The substring starting at 9 is `"barthefoo"`. It is the concatenation of `["bar","the","foo"]`.  
The substring starting at 12 is `"thefoobar"`. It is the concatenation of `["the","foo","bar"]`.

**Constraints:**

- `1 <= s.length <= 104`
- `1 <= words.length <= 5000`
- `1 <= words[i].length <= 30`
- `s` and `words[i]` consist of lowercase English letters.

```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        
    }
};
```

# Solution 1

1. Map the strings from `words` to a unordered map `mp`
2. Iterate the string `s` starting from `n*len` (`n`: size of vector `words` `len`: length of each string in `words` )
3. For every iteration map the `n` strings of `len` from `i-n*len` to `i` into a temporary unordered map `temp` 
4. If `temp==mp` add `i-n*len` to result vector `res`
## CODE (Mine)

```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        int len=words[0].length();
        int n=words.size();
        if(s.length()<n*len){
            return {};
        }
        //sort(words.begin(),words.end());
        unordered_map<string,int> mp;
        for(auto &s:words){
            mp[s]++;
        }
        vector<int> res;
        for(int i=n*len;i<=s.length();i++){
            //vector<string> temp;
            unordered_map<string,int> temp;
            for(int j=i-n*len;j<i;j+=len){
                //temp.push_back(string(s.begin()+(j),s.begin()+(j+len)));
                temp[string(s.begin()+(j),s.begin()+(j+len))]++;
            }
            //for(auto &x:temp){
            //    cout<<x<<" ";
            //}
            //sort(temp.begin(),temp.end());
            
            //cout<<"\n";
            if(mp==temp){
                res.push_back(i-n*len);
            }
        }
        return res;
    }
};
```

# Solution 2

**Input**:

**s** = "wordbestgoodwordword", **words** = ["word","good","best","word"]

let **w** be the length of each word in words array.  
let **m** be the length of words array.  
let **n** be the lenth of s string.

Now let's define a subproblem(i) as whether index i is present in our answer or not ? (let's take i = 0 for example)

Now, index i will be present in our answer if substring of s ( i, i+(m x w) ), if broken to w length pieces, contains all the words in the words array in any order.

![image](https://assets.leetcode.com/users/images/7c57e142-edca-44f5-889b-4697398bad03_1644254764.2690675.png)

So, in a standard brute force approach **how can we just solve subproblem(i) ???**

Create a hashmap storing what all is present in words array.  
Start from index i consumng w length m sub-strings, storing in another hashmap to keep track of what is observed.  
if two hash-maps match at the end, subproblem(i) tells i should be present in our answer.

Quick time complexity check for solving a single subproblem(i) is O(w x m)

_This is very easy, and I hope everyone reached till here._

Now, we can finally solve subproblem(i) like above for each possible i the s string, ie i can range from 0 to n-1,

Quick time complexity check for solving a single subproblem(i) is O(n x (w x m)). Now this is cubic complexity and we do not want this.

Now, always remember whenever we want to optimise an approach, we want to re-use already done computations.

### Can we re-use previously done computations in this problem ???

Let us assume we solved the sub-problem(i), where can be re-use it:

Now, while solving the sub-problem(i), we generate a hash-map of observed words. Can we reuse this hash-map for other sub-problems ??

![image](https://assets.leetcode.com/users/images/4b034119-e729-436d-8ebb-9ae0dc230ca7_1644255347.323178.png)

Now let's see how the sub-problem(i+1) looks ?

![image](https://assets.leetcode.com/users/images/f6c00157-6b27-4ce8-9aab-854c7598ea97_1644255670.64441.png)

Now the hash-map created in sub-problem(i) cannot be used in sub-problem(i+1) because w length words have completely changed now.

But, the hash-map created in sub-problem(i), can be re-used (after removing w length word starting with i) for sub-problem(i+w) because intermediate 3 words of length w remain same.

![image](https://assets.leetcode.com/users/images/1530055a-0e3c-4ca0-8040-14765662a1f4_1644255864.0705793.png)

So, computations made in subproblem(i) can be re-used in sub-problem(i+w).  
Similarly, computations made in subproblem(i+w) can be re-used in sub-problem(i+2 x w)

```rust
 i  ---> i+w ---> i+2w ----> i+3w ----> i+4w  (These all can re-use their computations)
 
```

Quick, time complexity check:  
solving for sub-problems i, i+w, i+2xw,..... sequentially would require O(n) time complexity.  
for solving sub-problem(i), it took us O(m x w).  
for next sub-problem(i+w), it took us just O(w), removing initial w character, adding latter w character.  
so, to solve all problems, we effectively consume all array elements atmost O(1)  
So, total time complexity for this is O(n)  
Similarly, we can solve many such subproblems together, using computations of previous sub-problems

These all go together re-using previous computations: _(this alone is a standard sliding window problem)_

```rust
        i  ---> i+w ---> i+2w ----> i+3w ----> i+4w 
		
```

These all go together re-using previous computations: _(this alone is a standard sliding window problem)_

```lisp
        (i+1)  ---> (i+1)+w ---> (i+1)+2w ----> (i+1)+3w ----> (i+1)+4w 
```

These all go together re-using previous computations: _(this alone is a standard sliding window problem)_

```lisp
        (i+2)  ---> (i+2)+w ---> (i+2)+2w ----> (i+2)+3w ----> (i+2)+4w 
		
```

These all go together re-using previous computations: _(this alone is a standard sliding window problem)_

```lisp
        (i+3)  ---> (i+3)+w ---> (i+3)+2w ----> (i+3)+3w ----> (i+3)+4w 
		
```

Now, we realise all the subproblems are solved: because i+4 was solved in first iteration as w here is 4.

So, we need to do w such iterations above and each iteration take O(n) time. So, total time complexity is O(n x w).

## CODE
```cpp
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        unordered_map<string, int> dict;
        for(auto &w: words) dict[w]++;
        vector<int> res;
        int n = s.length(), m = words[0].length(), w = words.size();
        
        for(int k=0; k<m; k++) {                     // O(m)
            unordered_map<string, int> seen;
            int left = k, currLen = 0;
            for(int i=left; i+m<=n; i+=m) {          // 2*O(n/m)
                string temp = s.substr(i, m);        // O(m)
                if(!(dict.count(temp))) {
                    seen.clear();
                    currLen = 0;
                    left = i+m;
                } else {
                    seen[temp]++;
                    currLen++;
                    if(seen[temp] > dict[temp]) {
                        while(seen[temp] > dict[temp]) {
                            string temp1 = s.substr(left, m);
                            seen[temp1]--;
                            currLen--;
                            left += m;
                        }
                    }
                }
                if(currLen == w) {
                    res.push_back(left);
                    seen[s.substr(left, m)]--;
                    currLen--;
                    left += m;
                }
            }
        }
        return res;
    }
};
```