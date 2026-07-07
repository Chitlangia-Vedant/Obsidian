# [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

Given a string `s` containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.

``**Example 1:**

**Input:** s = "()"

**Output:** true

**Example 2:**

**Input:** s = "()\[]{}"

**Output:** true

**Example 3:**

**Input:** s = "(]"

**Output:** false

**Example 4:**

**Input:** s = "(\[])"

**Output:** true

**Example 5:**

**Input:** s = "(\[)]"

**Output:** false
``
**Constraints:**

- `1 <= s.length <= 104`
- `s` consists of parentheses only `'()[]{}'`.

```cpp
class Solution {
public:
    bool isValid(string s) {
        
    }
};
```

# Understanding

```
"{[()]}": Balanced

"{([)]}": Not Balanced (Equal Number of Opening and Closing Brackets)

"{[(][)]}": Not Balanced (Equal Number of Opening and Closing Brackets)
```


2 Things:
- Equal Number of Opening and Closing Brackets
- They Should be in Correct Order

# Solution:

Edge Cases:

(1) "" ----> Balanced/Valid

(2) 
```
if (s.length()%2 != 0)
		return false;
```

TRICK:

```
"{[()]}": Balanced

"{([)]}": Not Balanced (Equal Number of Opening and Closing Brackets)


"{[()]}"


Opening: {, [, (
Closing: ), ], }
```

(1) Same Kind of Opening and Closing Bracket - if/else  or Switch Condition

```
 { == }
 [ == ]
 ( == )
```


(2) Order of Opening Bracket == "Reverse" of Order of Closing Bracket 
- Balanced, Else Not Balanced

# Approach:

STACK:

Iterate Over String:

(1) Opening Brackets - Push to Stack
(2) Closing Brackets - Pop from Stack

AND

Compare with the Closing Brackets
(Popped Opening Bracket from Stack and Compare Closing Bracket)

It its same - continue
else, return false

Complete String Traversed: No Brackets left in Stack, return true
Else, return false

# DRU RUN:

```
"{[()]}"
OP: true

STACK:

( - TOP           [ - TOP        { - TOP      Empty
[				  {
{


pop: ( == ) : TRUE - continue

pop: [ == ] : TRUE - continue

pop: { == } : TRUE - continue


Complete String Traversed and Stack is Empty

OP: true
```

```
"{[(])}"
OP: false


STACK:

( - TOP         [ - TOP
[				{
{


pop: ( == ] : FALSE
OP: false
```

```
Input: "{[(])}..............................."
Output: false

In Case of Valid, need to check till complete end of String
```

```
Input: "}......."
Output: false
```

# CODE:

```Java
class Solution {
    public boolean isValid(String s) 
    {
        // Edge Cases:

        // Empty Case - return true
        if (s == "")
            return true;

        // Odd length - return false
        if (s.length()%2!=0)
            return false;
        
        // First char is closing- return false
        if (s.charAt(0) == ')' || s.charAt(0) == ']' || s.charAt(0) == '}')
            return false;


        Stack<Character> stack = new Stack<Character>();
        int i = 0;

        for (i=0; i<s.length(); i++)
        {
            char c = s.charAt(i);

            if (c == '[' || c == '{' || c == '(')
            {
                stack.push(c);
                continue;
            }

            // If Stack is Empty, return false
            if (stack.isEmpty())
                return false;
        
            char check;
            switch(c)
            {
                case ')':
                    check = stack.peek();
                    if (check != '(')
                        return false;
                    stack.pop();
                    break;
                
                case '}':
                    check = stack.peek();
                    if (check != '{')
                        return false;
                    stack.pop();
                    break;
                
                case ']':
                    check = stack.peek();
                    if (check != '[')
                        return false;
                    stack.pop();
                    break; 
            }
        }

        return stack.isEmpty(); // Complete String Traversed and Stack is Empty
    }
}
```

TC: O(N)
SC: O(N)