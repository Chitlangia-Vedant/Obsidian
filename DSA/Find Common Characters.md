# [1002. Find Common Characters](https://leetcode.com/problems/find-common-characters/)

Given a string array `words`, return _an array of all characters that show up in all strings within the_ `words` _(including duplicates)_. You may return the answer in **any order**.

**Example 1:**

**Input:** words = ["bella","label","roller"]
**Output:** ["e","l","l"]

**Example 2:**

**Input:** words = ["cool","lock","cook"]
**Output:** ["c","o"]

**Constraints:**

- `1 <= words.length <= 100`
- `1 <= words[i].length <= 100`
- `words[i]` consists of lowercase English letters.

# Solution
## Intuition

The problem requires finding common characters that appear in all strings within the array, including duplicates. The approach leverages frequency counting for each character in the alphabet (from 'a' to 'z'). By comparing the character frequencies across all words, we can identify which characters appear in all the words and how many times they appear.

## Approach

1. **Initialize Frequency Array:**
    
    - Start by counting the frequency of each character in the first word. This will be our reference frequency array.
2. **Update Frequency Array:**
    
    - For each subsequent word, count the frequency of each character.
    - Update the reference frequency array to keep only the minimum frequency of each character seen so far. This ensures that the reference frequency array will ultimately represent the characters that are common to all words and the minimum number of times they appear in any word.
3. **Build Result:**
    
    - Iterate through the reference frequency array.
    - For each character that has a non-zero frequency, add it to the result list the number of times it appears in the reference frequency array.

## Step-by-Step Explanation

Let's use the example input: `words = ["bella", "label", "roller"]`

### Step 1: Initialize Frequency Array

First, count the frequency of each character in the first word `"bella"`.

|Character|Frequency|
|---|---|
|a|1|
|b|1|
|e|1|
|l|2|
|-|0|

### Step 2: Update Frequency Array with Each Subsequent Word

#### Word 2: `"label"`

Count the frequency of each character in `"label"`:

|Character|Frequency|
|---|---|
|a|1|
|b|1|
|e|1|
|l|2|
|-|0|

Take the minimum frequency of each character between `"bella"` and `"label"`:

|Character|Frequency in "bella"|Frequency in "label"|Updated Frequency|
|---|---|---|---|
|a|1|1|1|
|b|1|1|1|
|e|1|1|1|
|l|2|2|2|
|-|0|0|0|

#### Word 3: `"roller"`

Count the frequency of each character in `"roller"`:

|Character|Frequency|
|---|---|
|e|1|
|l|2|
|o|1|
|r|2|
|-|0|

Update the frequency array again by taking the minimum:

|Character|Frequency in "label"|Frequency in "roller"|Updated Frequency|
|---|---|---|---|
|a|1|0|0|
|b|1|0|0|
|e|1|1|1|
|l|2|2|2|
|-|0|0|0|

### Step 3: Build Result

From the final frequency array, build the result list:

|Character|Frequency|Result List|
|---|---|---|
|e|1|["e"]|
|l|2|["e", "l", "l"]|
|-|0||

### Final Result

The common characters that show up in all strings within the array are `["e", "l", "l"]`.

### Summary Table

Here’s a summary of the frequency count after processing each word:

|Character|Initial ("bella")|After "label"|After "roller"|
|---|---|---|---|
|a|1|1|0|
|b|1|1|0|
|e|1|1|1|
|l|2|2|2|
|o|0|0|0|
|r|0|0|0|
|-|0|0|0|

## Complexity

- **Time Complexity:**
    
    - Counting characters in each word: O(n * m), where `n` is the number of words and `m` is the average length of the words.
    - Updating the reference frequency array: O(n * 26), since we compare 26 possible characters for each word.
    - Constructing the result list: O(26), iterating through the frequency array to build the result.
    - Overall, the time complexity is O(n * m), where `n` is the number of words and `m` is the length of the longest word.
- **Space Complexity:**
    
    - The space required for the frequency arrays is O(26) for each word.
    - Additional space for the result list depends on the number of common characters, which is O(m) in the worst case (though typically much smaller).
    - Overall, the space complexity is O(26) + O(m) = O(m).

## CODE

```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<int> last = count(words[0]);
        for (int i = 1; i < words.size(); i++) {
            last = intersection(last, count(words[i]));
        }
        
        vector<string> result;
        for (int i = 0; i < 26; i++) {
            while (last[i] > 0) {
                result.push_back(string(1, 'a' + i));
                last[i]--;
            }
        }
        
        return result;
    }
    
private:
    vector<int> count(const string& str) {
        vector<int> frequency(26, 0);
        for (char c : str) {
            frequency[c - 'a']++;
        }
        return frequency;
    }
    
    vector<int> intersection(const vector<int>& a, const vector<int>& b) {
        vector<int> t(26, 0);
        for (int i = 0; i < 26; i++) {
            t[i] = min(a[i], b[i]);
        }
        return t;
    }
};
```