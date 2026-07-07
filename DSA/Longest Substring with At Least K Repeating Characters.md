# [395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)

Given a string `s` and an integer `k`, return _the length of the longest substring of_ `s` _such that the frequency of each character in this substring is greater than or equal to_ `k`.

if no such substring exists, return 0.

**Example 1:**

**Input:** s = "aaabb", k = 3
**Output:** 3
**Explanation:** The longest substring is "aaa", as 'a' is repeated 3 times.

**Example 2:**

**Input:** s = "ababbc", k = 2
**Output:** 5
**Explanation:** The longest substring is "ababb", as 'a' is repeated 2 times and 'b' is repeated 3 times.

**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of only lowercase English letters.
- `1 <= k <= 105`

```cpp
class Solution {
public:
    int longestSubstring(string s, int k) {
        
    }
};
```
# Solution 

1. Count the frequency of every character in the given string using an unordered map `m`.
2. Initialize a variable `prev` to `0` to mark the starting index of the current substring and a variable `mx` to store the maximum valid substring length.
3. Traverse the string from left to right.
4. For each character, check whether its frequency in the current string is less than `k`.
5. If such a character is found, it cannot be part of any valid substring. Recursively compute the longest valid substring in the segment from index `prev` to `i-1` using `s.substr(prev, i-prev)`.
6. Update `mx` with the maximum of its current value and the value returned by the recursive call.
7. Set `prev = i + 1` so that the next segment starts after the invalid character.
8. After the traversal, if `prev` is still `0`, it means every character in the current string appears at least `k` times. Return the length of the current string.
9. Otherwise, recursively compute the longest valid substring in the last segment `s.substr(prev)` and update `mx`.
10. Return `mx` as the length of the longest substring in which every character appears at least `k` times.
## CODE (Mine)

```cpp
class Solution {
public:
    int longestSubstring(string s, int k) {
        cout<<s<<"\n";
        unordered_map<char,int> m;
        for(int i=0;i<s.length();i++){
            m[s[i]]++;
        }
        int prev=0;
        int mx=0;
        for(int i=0;i<s.length();i++){
            if (m[s[i]]<k){
                mx = max(mx,longestSubstring(s.substr(prev, i-prev), k));
                prev = i + 1;
            }
        }
        if (prev == 0) {
            return s.length();
        }
        // Last segment
        mx = max(mx,longestSubstring(s.substr(prev), k));
        return mx;
    }
};
```

# Solution 2: Naïve Approach

Consider all the possible sub-strings and for every sub-string, calculate the frequency of each of its character and check whether all the characters appear at least K times. For all such valid sub-strings, find the largest length possible.  
Below is the implementation of the above approach:

# Complexity

- Time complexity:O($n^2$)
- Space complexity:O(MAX), where MAX is the maximum number of unique characters in the string.

# Code

```csharp
class Solution {
public:
    #define MAX 26

    // Function that return true if frequency
    // of all the present characters is at least k
    bool atLeastK(int freq[], int k)
    {
        for (int i = 0; i < MAX; i++) {
    
            // If the character is present and
            // its frequency is less than k
            if (freq[i] != 0 && freq[i] < k)
                return false;
        }
    
        return true;
    }

    void setZero(int freq[])
    {
        for (int i = 0; i < MAX; i++)
            freq[i] = 0;
    }


    int longestSubstring(string s, int k) {
        int n=s.length();
        int maxLen = 0;
 
        int freq[MAX];
    
        // Starting index of the sub-string
        for (int i = 0; i < n; i++) {
            setZero(freq);
    
            // Ending index of the sub-string
            for (int j = i; j < n; j++) {
                freq[s[j] - 'a']++;
    
                // If the frequency of every character
                // of the current sub-string is at least k
                if (atLeastK(freq, k)) {
    
                    // Update the maximum possible length
                    maxLen = max(maxLen, j - i + 1);
                }
            }
        }
    
        return maxLen;
    }
};
```

# Solution 3: Sliding Window

## Complexity

- Time complexity: O(N)
- Space complexity: O(1)

## CODE

```java
class Solution {
public:
 

// Function to find the length of
// the longest substring
int longestSubstring(string s, int k)
{
	// Store the required answer
	int ans = 0;

	// Create a frequency map of the
	// characters of the string
	int freq[26] = { 0 };

	// Store the length of the string
	int n = s.size();

	// Traverse the string, s
	for (int i = 0; i < n; i++)

		// Increment the frequency of
		// the current character by 1
		freq[s[i] - 'a']++;

	// Stores count of unique characters
	int unique = 0;

	// Find the number of unique
	// characters in string
	for (int i = 0; i < 26; i++)
		if (freq[i] != 0)
			unique++;

	// Iterate in range [1, unique]
	for (int curr_unique = 1;
		curr_unique <= unique;
		curr_unique++) {

		// Initialize frequency of all
		// characters as 0
		memset(freq, 0, sizeof(freq));

		// Stores the start and the
		// end of the window
		int start = 0, end = 0;

		// Stores the current number of
		// unique characters and characters
		// occurring atleast K times
		int cnt = 0, count_k = 0;

		while (end < n) {
			if (cnt <= curr_unique) {
				int ind = s[end] - 'a';

				// New unique character
				if (freq[ind] == 0)
					cnt++;

				freq[ind]++;

				// New character which
				// occurs atleast k times
				if (freq[ind] == k)
					count_k++;

				// Expand window by
				// incrementing end by 1
				end++;
			}
			else {
				int ind = s[start] - 'a';

				// Check if this character
				// is present atleast k times
				if (freq[ind] == k)
					count_k--;

				freq[ind]--;

				// Check if this character
				// is unique
				if (freq[ind] == 0)
					cnt--;

				// Shrink the window by
				// incrementing start by 1
				start++;
			}

			// If there are curr_unique
			// characters and each character
			// is atleast k times
			if (cnt == curr_unique
				&& count_k == curr_unique)

				// Update the overall
				// maximum length
				ans = max(ans, end - start);
		}
	}

	// return the answer
	return ans;
    }
};
```