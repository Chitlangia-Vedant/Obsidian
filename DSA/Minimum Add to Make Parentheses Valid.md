[921. Minimum Add to Make Parentheses Valid](https://leetcode.com/problems/minimum-add-to-make-parentheses-valid/)

A parentheses string is valid if and only if:

- It is the empty string,
- It can be written as `AB` (`A` concatenated with `B`), where `A` and `B` are valid strings, or
- It can be written as `(A)`, where `A` is a valid string.

You are given a parentheses string `s`. In one move, you can insert a parenthesis at any position of the string.

- For example, if `s = "()))"`, you can insert an opening parenthesis to be `"(**(**)))"` or a closing parenthesis to be `"())**)**)"`.

Return _the minimum number of moves required to make_ `s` _valid_.

**Example 1:**

**Input:** s = "())"
**Output:** 1

**Example 2:**

**Input:** s = "((("
**Output:** 3

**Constraints:**

- `1 <= s.length <= 1000`
- `s[i]` is either `'('` or `')'`.

```cpp
class Solution {
    public int minAddToMakeValid(String s) {
        
    }
}
```

# Understanding:

Minimum "Add" to Make Parentheses Valid

- Can Add ( or )
- Can Add Anywhere

```
"())" -----> "(())"

1(  Add

OP: 1
```

```
"()"
OP: 0
```

```
"((("
OP: 3 (3 Closing Brackets)
```

```
")))"
OP: 3 (3 Opening Brackets)
```

```
")(" ----> "()()"
OP: 2
```

```
"()()"
OP: 0
```
# Solution:

Incorrect Approach: No Order Maintained

( - count a
) - count b

return abs(a-b);

````
"()))(("
opening = 3
closing = 3
ans = abs(3-3) = 0
Expected OP: 4
````
# Solution:

(1) With Stack:
TC: O(N), SC: O(N)

(2) Without Stack:
TC: O(N), SC: O(1)

# Approach:

Two Pointer,
open = 0 and close = 0

```
if (s[i] == '(')
	++close;

else if (close > 0)
	--close;

else 
	++open;
	
return open + close;
```

## Detailed Approach:

### (1) Found '(' ----> ++close
Must Add Closing Bracket to make it Balanced

### (2) Found ')'
2 Cases:
#### (A) close > 0 ------> --close
Sufficient Closing Bracket, using that
#### (B) Else ---> ++open
Increase the Opening Bracket

return open + close
# DRY RUN:

```
"()" OP: 0
open = 0
close = 0 -> 1 -> 0

return open + close = 0
```

```
")))" OP: 3
open = 0 -> 1 -> 2 -> 3
close = 0

return open + close = 3
```

```
"())((" OP: 3
open = 0 -> 1 
close = 0 -> 1 -> 0 -> 1 -> 2

return open + close = 3
```

# CODE:

```Java
class Solution {
    public int minAddToMakeValid(String s) 
    {
        int open = 0, close = 0;
        int len = s.length();
        int i = 0;

        for (i=0; i<len; i++)
        {
            if (s.charAt(i) == '(')
                ++close;

            else if (close > 0)
                --close;

            else
                ++open;
        }

        return open + close;
    }
}
```

T: O(N), S: O(1)

# Approach - 1 : Stack Based Solution
// T: O(N), S: O(N)

```Java
class Solution {
    public int minAddToMakeValid(String s) 
    {
        if (s.length() == 0)
            return 0;

        Stack<Character> stack = new Stack<>();
        int i = 0;
        for (i=0; i<s.length(); i++)
        {
            // Opening Bracket
            if (s.charAt(i) == '(')
                stack.push(s.charAt(i));

            // Closing Bracket - Pop
            if (s.charAt(i) == ')')
            {
                if (stack.size()> 0 && stack.peek() =='(')
                    stack.pop();

                else
                    stack.push(s.charAt(i));
            }
        }

        return stack.size();
    }
}
```
