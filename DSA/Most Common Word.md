# [819. Most Common Word](https://leetcode.com/problems/most-common-word/)

Given a string `paragraph` and a string array of the banned words `banned`, return _the most frequent word that is not banned_. It is **guaranteed** there is **at least one word** that is not banned, and that the answer is **unique**.

The words in `paragraph` are **case-insensitive** and the answer should be returned in **lowercase**.

**Note** that words can not contain punctuation symbols.

**Example 1:**

**Input:** paragraph = "Bob hit a ball, the hit BALL flew far after it was hit.", banned = ["hit"]
**Output:** "ball"
**Explanation:** 
"hit" occurs 3 times, but it is a banned word.
"ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
Note that words in the paragraph are not case sensitive,
that punctuation is ignored (even if adjacent to words, such as "ball,"), 
and that "hit" isn't the answer even though it occurs more because it is banned.

**Example 2:**

**Input:** paragraph = "a.", banned = []
**Output:** "a"

**Constraints:**

- `1 <= paragraph.length <= 1000`
- paragraph consists of English letters, space `' '`, or one of the symbols: `"!?',;."`.
- `0 <= banned.length <= 100`
- `1 <= banned[i].length <= 10`
- `banned[i]` consists of only lowercase English letters.

```cpp
class Solution {
public:
    string mostCommonWord(string paragraph, vector<string>& banned) {
        
    }
};
```

# Understanding:

1. Ignore punctuations
	"balls," -----> "balls"
2. Ignore Extra Characters and Spaces
	"!?',;." and " "
3. Ignore Upper Case and Lower Case
4. Check Word is Not Banned
5. Find Most Frequent Word: Count

It is guaranteed there is at least one word that is not banned, and that the answer is unique.
# Solution:

## (1) Convert paragraph into Array of Strings

paragraph ------> String[] words

Remove Extra Characters ("!?',;.") and Spaces (" ")
- Using Regex
- Using Split
## (2) Convert All String in String\[] words to lowercase:

Constraints:
banned\[i] consists of only lowercase English letters.
- I have to match the words present in words\[] NOT present in banned\[]
## (3) String[] words and String[] banned

Ques: Find Most Common word present in words[], not present in banned[]

Best: Set

Lookup Time for Array: O(N)
Lookup Time for Set: O(1)

Convert Banned Array into Set and lookup over banned[] in O(1)

## (4) Highest Frequency ----> Map

Map:

Key: Word
Value: Frequency

# CODE:

```Java
class Solution {
    public String mostCommonWord(String paragraph, String[] banned) 
    {
    // Create Set out of banned Array, lookup - O(1)
    Set<String> banSet = new HashSet<String>(Arrays.asList(banned));

    // hashmap for frequency of most common word
    Map<String, Integer> count = new HashMap<String, Integer>();

    // Create words array using regex and convert to lowercase
    // Constraints: banned[i] consists of only lowercase English letters.
    // "ball." -> "ball" -> [ball]

    String[] words = paragraph.replaceAll("\\W+", " ").toLowerCase().split("\\s+"); // O(N) 

    for (String word: words) // O(N)
    {
        // if word is not banned, put into map
        if(!banSet.contains(word)) // O(1)
            {
                count.put(word, count.getOrDefault(word, 0) + 1);
            }
    }

    // Return the key with Highest Frequency (Value)
    return Collections.max(count.entrySet(), Map.Entry.comparingByValue()).getKey();
    }
}
```

TC: O(N)
SC: O(N) - Set + Map

# CODE

```cpp
class Solution {
public:
    string mostCommonWord(string p, vector<string>& banned) {
        unordered_set<string> ban(banned.begin(), banned.end());
        unordered_map<string, int> count;
        for (auto & c: p) c = isalpha(c) ? tolower(c) : ' ';
        istringstream iss(p);
        string w;
        pair<string, int> res ("", 0);
        while (iss >> w)
            if (ban.find(w) == ban.end() && ++count[w] > res.second)
                res = make_pair(w, count[w]);
        return res.first;
    }
};
```