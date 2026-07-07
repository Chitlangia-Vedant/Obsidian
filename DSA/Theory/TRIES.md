#  Introduction

- Data Structure used to store a set of keys 
Set of Keys - Strings, Integers etc

Purpose:
- Efficient Retrieval and Storage of Keys
- Highly useful for LARGE Datasets

Operations: 
- Insertion
- Deletion
- Search
- Prefix Search
# Representation of Trie Node

- Consists of: Nodes connected by Edges
- Node: Represents a character/part of string
- Root Node: Does not contain any char

```
			Root
		| | | | | | 	
		a     .....z 26
	| | | | 	
	a......z 26
```

At Every Node, 
There is a boolean which is "endOfWord" which can be true/false

```
Root
 |
 a  (endOfWord = 0)
 |
 n  (endOfWord = 0)
 |
 d  (endOfWord = 1)
```

# CODE:

```
// Structure of Node a Trie
class TrieNode
{
	TrieNode[] children; // Self Referential Object
	boolean isEndofWord;

	public TrieNode
	{
		isEndofWord = false;
		children = new TrieNode[26]; // Self Referential Object
	}
}
```

26 because: a-z chars

# Applications:

- Autocomplete
Type: a, Suggest: an, and, ant 

- Spell Checker
- Prefix Search
- IP Routing (Longest Prefix Matching)
- T9 Predictive Text

# Insertion in Trie Data Structure

Insert 2 words in sequence:
1 - and
2 - ant

Initially, children`[26]`: All are null values

1st: Insert "and"
- Start at Root Node, isEndofWord = 0
- First character = 'a', Index of 'a' = 'a'-'a' = 0
child[0] = 'a' if its null, isEndofWord = 0
- Second character = 'n', Index of 'n' = 'n'-'a' = 13
child[13] = 'n' if its null, isEndofWord = 0
- Third character = 'd', Index of 'd' = 'd'-'a' = 4
child[4] = 'd' if its null, isEndofWord = 1 -> END of Word


2nd: Insert "ant" AFTER "and" is already there
- Start at Root Node, isEndofWord = 0
- First character = 'a', Index of 'a' = 'a'-'a' = 0
child[0] = 'a' if its null, It is NOT NULL
We use the EXISTING 'a', isEndofWord = 0
- Second character = 'n', Index of 'n' = 'n'-'a' = 13
child[13] = 'n' if its null, It is NOT NULL
We use the EXISTING 'n', isEndofWord = 0
- Third character = 't', Index of 't' = 't'-'a' = 19
child[19] = 't' if its null, isEndofWord = 1 -> END of Word

Complexity of Insertion:

TC: O(N), N: Length of Word
SC: O(N), N: Length of Word

# Searching in Trie Data Structure

- Search is similar operation as Insert
- Take the char and move down in the Trie to search and match
- 2 Possibilities:
Word Found: return true
Not Found: return false

Case-1: Present

```
Root
 |
 a  (endOfWord = 0)
 |
 n  (endOfWord = 0)
 |
 d  (endOfWord = 1)
```


Search: "and"
- Start from root node
- Go to branch corresponding to "a"
- Go to branch corresponding to "n"
- Go to branch corresponding to "d", Flag: isEndofWord = 1
Present in Trie -> Return true


Case-2: Not Present

```
Root
 |
 a  (endOfWord = 0)
 |
 n  (endOfWord = 0)
 |
 d  (endOfWord = 1)
```

Search: "ant"

- Start from root node
- Go to branch corresponding to "a"
- Go to branch corresponding to "n"
- Go to branch corresponding to "t", Value = null
Not Present in Trie -> Return false

Complexity of Search:

TC: O(N), N: Length of Word
SC: O(1)
# Implementation of Insert and Search Operations in Trie - Final Code

```
import java.util.Arrays;
import java.util.List;

class TrieNode {
    TrieNode[] child = new TrieNode[26];
    boolean wordEnd = false;
}

public class Main {

    static void insertKey(TrieNode root, String key) {
        TrieNode curr = root;
        for (char c : key.toCharArray()) {
            if (curr.child[c - 'a'] == null) {
                TrieNode newNode = new TrieNode();
                curr.child[c - 'a'] = newNode;
            }
            curr = curr.child[c - 'a'];
        }
        curr.wordEnd = true;
    }

    static boolean searchKey(TrieNode root, String key) {
        TrieNode curr = root;
        for (char c : key.toCharArray()) {
            if (curr.child[c - 'a'] == null) 
                return false;
            curr = curr.child[c - 'a'];
        }
        return curr.wordEnd;
    }

    public static void main(String[] args) {
        TrieNode root = new TrieNode();
        List<String> arr = Arrays.asList(
            "and", "ant", "do", "geek", "dad", "ball");
        for (String s : arr) {
            insertKey(root, s);
        }

        List<String> searchKeys = 
              Arrays.asList("do", "gee", "bat", "ball", "ant", "an");
        for (String s : searchKeys) {
            System.out.println("Key : " + s);
            if (searchKey(root, s)) 
            {
                System.out.println("Present");
                System.out.println("");
            }
            else 
            {
                System.out.println("Not Present");                   
                System.out.println("");
            }
        }
    }
}
```

```Output
Key : do
Present

Key : gee
Not Present

Key : bat
Not Present

Key : ball
Present

Key : ant
Present

Key : an
Not Present
```

TC: O(N) - Insert, Search
SC: O(N) - Insert
