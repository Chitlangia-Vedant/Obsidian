# [1720. Decode XORed Array](https://leetcode.com/problems/decode-xored-array/)

There is a **hidden** integer array `arr` that consists of `n` non-negative integers.

It was encoded into another integer array `encoded` of length `n - 1`, such that `encoded[i] = arr[i] XOR arr[i + 1]`. For example, if `arr = [1,0,2,1]`, then `encoded = [1,2,3]`.

You are given the `encoded` array. You are also given an integer `first`, that is the first element of `arr`, i.e. `arr[0]`.

Return _the original array_ `arr`. It can be proved that the answer exists and is unique.

**Example 1:**

**Input:** encoded = [1,2,3], first = 1
**Output:** [1,0,2,1]
**Explanation:** If arr = [1,0,2,1], then first = 1 and encoded = [1 XOR 0, 0 XOR 2, 2 XOR 1] = [1,2,3]

**Example 2:**

**Input:** encoded = [6,2,7,3], first = 4
**Output:** [4,2,0,7,4]

**Constraints:**

- `2 <= n <= 104`
- `encoded.length == n - 1`
- `0 <= encoded[i] <= 105`
- `0 <= first <= 105`

```
class Solution {
public:
    vector<int> decode(vector<int>& encoded, int first) {
        
    }
};
```

# Understanding:

Orig: [1 2 3 4]
Encoded Array of XOR of Adjacent Values
= [1^2, 2^3, 3^4]
= [val1, val2, val3]
Orig Array: N
Encoded Array with XOR of Adjacent Values 
Encoded Array Size: N-1

encoded[i] = arr[i] ^ arr[i+1]

Given: Encoded Array: encoded[]
Find: Original Array: arr[]

# Solution:

```
encoded[i] = arr[i] ^ arr[i+1]
arr[i] = encoded[i] (Inverse of XOR) arr[i+1]
```

#IMP 
```
According to XOR Property:
The Inverse of XOR is XOR Itself

If you have 
c = a ^ b

Need to find a:
a = c^b = b^c

Need to find b:
a = c^a = a^c
```

**CODE:**
```cpp
public class Main 
{
    public static void main(String[] args) 
    {
        int a = 4, b =8, c;
        c = a^b;
        
        int ans = c^b; // a = c^b
        System.out.println(ans == a);
        
        ans = c^a; // b = c^a
        System.out.println(ans == b);   
    }
}
```

```Output
true
true
```

```
encoded[i] = arr[i] ^ arr[i+1]
arr[i+1] = encoded[i] ^ arr[i] = arr[i] ^ encoded[i]
arr[0] = first
```

# CODE:

```cpp
    public int[] decode(int[] encoded, int first) 
    {
        int n = encoded.length;
        int[] orig = new int[n+1];
        
        orig[0] = first;
        
        int i = 0;
        for (i=0; i<n; i++)
            orig[i+1] = orig[i] ^ encoded[i];
            //arr[i+1] = arr[i] ^ encoded[i] 
                  
        return orig;
    }
```


TC: O(N)
SC: O(1)
