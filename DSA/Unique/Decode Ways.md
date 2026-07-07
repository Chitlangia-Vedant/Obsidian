# [91. Decode Ways](https://leetcode.com/problems/decode-ways/)

You have intercepted a secret message encoded as a string of numbers. The message is **decoded** via the following mapping:

`"1" -> 'A'   "2" -> 'B'   ...   "25" -> 'Y'   "26" -> 'Z'`

However, while decoding the message, you realize that there are many different ways you can decode the message because some codes are contained in other codes (`"2"` and `"5"` vs `"25"`).

For example, `"11106"` can be decoded into:

- `"AAJF"` with the grouping `(1, 1, 10, 6)`
- `"KJF"` with the grouping `(11, 10, 6)`
- The grouping `(1, 11, 06)` is invalid because `"06"` is not a valid code (only `"6"` is valid).

Note: there may be strings that are impossible to decode.  
  
Given a string s containing only digits, return the **number of ways** to **decode** it. If the entire string cannot be decoded in any valid way, return `0`.

The test cases are generated so that the answer fits in a **32-bit** integer.

**Example 1:**

**Input:** s = "12"

**Output:** 2

**Explanation:**

"12" could be decoded as "AB" (1 2) or "L" (12).

**Example 2:**

**Input:** s = "226"

**Output:** 3

**Explanation:**

"226" could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

**Example 3:**

**Input:** s = "06"

**Output:** 0

**Explanation:**

"06" cannot be mapped to "F" because of the leading zero ("6" is different from "06"). In this case, the string is not a valid encoding, so return 0.

**Constraints:**

- `1 <= s.length <= 100`
- `s` contains only digits and may contain leading zero(s).

```cpp
class Solution {
public:
    int numDecodings(string s) {
        
    }
};
```

# Approach

## How we think about a solution

```javascript
Input = "2215"
```

initialize array for dynamic programming with length of input string + 1. 

And update index 0 and index 1 with 1.

```javascript
dp = [0,0,0,0,0]
dp = [1,1,0,0,0]

index0: ("")
index1: (2)
```

Index 0 is for empty string. That is one pattern.  

Index 1 is for the first character(= 2). That is one pattern, because we cannot create two digits.

Now we are at index 2, it's easy to get one digit and two digits. just subtract 1 from current index for one digits and get characters between current index - 2 and current index for two digits.

```python
one = int(s[i - 1]) # one digit
two = int(s[i - 2:i]) # two digits
```

```javascript
Input = "2215"
          ↑
```

In this case, let's consider input string as "22". We can create one digit and two digits which are "2" and "22".

"2" is coming from index 1 in the input string. We can create a combination with "2" at index 0 in the input string. Update index 2 in dp array with index 1 in dp array. Because "2" at index 1 is only one pattern.

```javascript
dp = [1,1,1,0,0]

index0: ("")
index1: (2)
index2: (2,2)
```

We have two digits(= "22"). In this case, we use the first two characters, so combination should be ("22"). That is kind of empty and "22" combination, so add index 0 in dp to index 2. This is a reason why we add + 1 when we create dp array.

```javascript
dp = [1,1,2,0,0]

index0: ("")
index1: (2)
index2:(2,2), (22)
```

---

Go next. Let's consider input string as "221". We will create "1" and "21".

```javascript
Input = "2215"
            ↑

Possible combination with "1" should be
(2,2), (22) → (2,2,1), (22,1)

Possible combination with "21" should be
(2) → (2,21)

dp = [1,1,2,3,0]

index0: ("")
index1: (2)
index2: (2,2), (22)
index3: (2,2,1), (22,1), (2,21)
```

---

⭐️ Points

As you can see, possible combination with one digit should be current index - 1, because we only use current character for one digit, so all previous combinations will be possible combinations.

possible combination with two digit should be current index - 2, because we use the latest two characters for two digits, so combinations before current index - 2 will be possible combinations.

---

Go next. Let's consider input string as "2215". We will create "5" and "15".

```javascript
Input = "2215"
             ↑

Possible combination with "5" should be
(2,2,1), (22,1), (2,21) → (2,2,1,5), (22,1,5), (2,21,5)

Possible combination with "15" should be
(2,2), (22) → (2,2,15), (22,15)

dp = [1,1,2,3,5]

index0: ("")
index1: (2)
index2: (2,2), (22)
index3: (2,2,1), (22,1), (2, 21)
index4: (2,2,1,5), (22,1,5), (2,21,5), (2,2,15), (22,15)
```

```
Output: 5
```

---

# Complexity

- Time complexity: O(n)

- Space complexity: O(n)

```python
class Solution {
public:
    int numDecodings(string s) {
        if (s[0] == '0') {
            return 0;
        }

        int n = s.length();
        vector<int> dp(n + 1, 0);
        dp[0] = dp[1] = 1;

        for (int i = 2; i <= n; i++) {
            int one = s[i - 1] - '0';
            int two = stoi(s.substr(i - 2, 2));

            if (1 <= one && one <= 9) {
                dp[i] += dp[i - 1];
            }
            if (10 <= two && two <= 26) {
                dp[i] += dp[i - 2];
            }
        }

        return dp[n];        
    }
};
```

## Step by step algorithm

##### Input Validation

```python
if s[0] == '0':
    return 0
```

- **Explanation:** Checks if the first character of the string is '0'. If it is, there's no valid decoding, and the function returns 0.

##### Initialization

```python
dp = [0] * (n + 1)
dp[0], dp[1] = 1, 1
```

- **Explanation:** Initializes an array `dp` of size `n + 1` to store the number of decodings for each position. Sets base cases: an empty string and a single character both have one decoding.

##### Iteration

```python
for i in range(2, n + 1):
```

- **Explanation:** The loop starts from the third character and goes up to the end of the string.

##### Extract Digits

```python
one = int(s[i - 1])
two = int(s[i - 2:i])
```

- **Explanation:** Extracts one-digit (`one`) and two-digit (`two`) integers from the string.

##### Count Ways for One-Digit

```python
if 1 <= one <= 9:
    dp[i] += dp[i - 1]
```

- **Explanation:** If `one` is between 1 and 9, adds the number of ways to decode one-digit to `dp[i]`.

##### Count Ways for Two-Digits

```python
if 10 <= two <= 26:
    dp[i] += dp[i - 2]
```

- **Explanation:** If `two` is between 10 and 26, adds the number of ways to decode two-digits to `dp[i]`.

##### Final Result

```python
return dp[n]
```

- **Explanation:** The final result is the number of ways to decode the entire string, which is stored in `dp[n]`.


