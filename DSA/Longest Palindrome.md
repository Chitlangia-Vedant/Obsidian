# [409. Longest Palindrome](https://leetcode.com/problems/longest-palindrome/)

Given a string `s` which consists of lowercase or uppercase letters, return the length of the **longest palindrome** that can be built with those letters.

Letters are **case sensitive**, for example, `"Aa"` is not considered a palindrome.

**Example 1:**

**Input:** s = "abccccdd"
**Output:** 7
**Explanation:** One longest palindrome that can be built is "dccaccd", whose length is 7.

**Example 2:**

**Input:** s = "a"
**Output:** 1
**Explanation:** The longest palindrome that can be built is "a", whose length is 1.

**Constraints:**

- `1 <= s.length <= 2000`
- `s` consists of lowercase **and/or** uppercase English letters only.

```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        
    }
};
```

# Approach

Example:  
**Input:** `s = "abccccdd"`  
**Output:** `7`

---

### Understanding the Nature of a Palindrome

A palindrome must have the same characters on the left and right sides.

That means if we have **two identical characters**, we can use them to extend the palindrome — one on the left and one on the right.

The problem is that we don’t know how many times each character appears in the input string. So the first step is to count the frequency of every character.

### Counting Characters

We could use either:

- An array
- A hash map (dictionary)

An array would require us to map each character to a specific index, which can be slightly tricky. So here, using a **hash map** is a cleaner and better choice.

After counting characters in:

```javascript
"abccccdd"
```

We get:

```
{ a: 1, b: 1, c: 4, d: 2 }
```

### Building the Palindrome Length

As mentioned earlier, every pair of identical characters can extend the palindrome. So now we iterate over the values in the hash map. We check each count using:

```erlang
count % 2
```

##### ・Why `% 2` Instead of `// 2`?

In almost all palindrome problems, you need to think about two cases:

1. Even length
2. Odd length

Using `% 2` helps us determine whether the count is even or odd.

##### Case 1: Even Count

If:

```erlang
count % 2 == 0
```

Then the entire count can be used in the palindrome.

For example:

```
c: 4
d: 2
```

All of them can be used.

##### Case 2: Odd Count

If the count is odd, like:

```
a: 5
```

We can only use 4 of them to form pairs.  
The last one does not have a matching pair.

Similarly:

```
a: 3
```

We can only use 2.

So whenever the count is odd, we do:

```
length += count - 1
```

This makes the number even so it can contribute fully to the palindrome sides.

### The Important Trick (The Final Catch)

Earlier I said that the leftover single character (from an odd count) cannot be used. But that is **not entirely true**. There is one exception:

A palindrome can have **exactly one character in the middle**.

For example:

```
acbcbca
```

Here:

- `c` appears 3 times.
- Two `c`s form a pair.
- The remaining `c` can sit in the center.

That single character without a pair can still increase the palindrome length by 1.

In our example:

```
{ a: 1, b: 1, c: 4, d: 2 }
```

Both `a` and `b` appear once. Normally, neither can form a pair. However, **one of them** can be placed in the center of the palindrome. That’s why:

If we find at least one odd count, we add:

```
+1
```

at the end.

### Final Algorithm Summary

1. Count the frequency of each character using a hash map.
    
2. For each count:
    
    - If even → add the full count.
    - If odd → add `count - 1` and remember that an odd exists.
3. If at least one odd count exists → add 1 at the end.
    

This guarantees we get the correct maximum palindrome length.

---

# Complexity

Based on Python.

- Time complexity: O(n)

- Space complexity: O(26)or O(52) → O(1)

# CODE (Mine)

```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        map<char,int> m;
        for(char c:s){
            m[c]++;
        }
        int count=0;
        bool odd=false;
        for(auto c:m){
            if(c.second%2==0)
                count+=c.second;
            else{
                count+=c.second-1;
                if(!odd){
                    count++;
                    odd=true;
                }
            }
        }
        return count;
    }
};
```