[3348. Smallest Divisible Digit Product II](https://leetcode.com/problems/smallest-divisible-digit-product-ii/)

You are given a string `num` which represents a **positive** integer, and an integer `t`.

A number is called **zero-free** if _none_ of its digits are 0.

Return a string representing the **smallest** **zero-free** number greater than or equal to `num` such that the **product of its digits** is divisible by `t`. If no such number exists, return `"-1"`.

**Example 1:**

**Input:** num = "1234", t = 256

**Output:** "1488"

**Explanation:**

The smallest zero-free number that is greater than 1234 and has the product of its digits divisible by 256 is 1488, with the product of its digits equal to 256.

**Example 2:**

**Input:** num = "12355", t = 50

**Output:** "12355"

**Explanation:**

12355 is already zero-free and has the product of its digits divisible by 50, with the product of its digits equal to 150.

**Example 3:**

**Input:** num = "11111", t = 26

**Output:** "-1"

**Explanation:**

No number greater than 11111 has the product of its digits divisible by 26.

**Constraints:**

- `2 <= num.length <= 2 * 105`
- `num` consists only of digits in the range `['0', '9']`.
- `num` does not contain leading zeros.
- `1 <= t <= 1014`
# Intuition

Every digit `1–9` contains only the prime factors `2,3,5,7`. So first factorize `t`. If it contains another prime, answer is `-1`.  
For example, `t=256=2⁸`, so the digits of our answer must collectively contain at least eight `2`s.  
If `num` is already zero-free and has enough factors, return it.  
Otherwise, we need the smallest number `>= num`. To get the smallest number, keep the longest possible prefix unchanged. So we go from right to left, try increasing the current digit, and then build the smallest possible suffix that supplies the remaining factors.  
For the suffix, we pack factors to use as few digits as possible: `2³→8`, `3²→9`, `2×3→6`, etc. Any unused positions are filled with `1`, since `1` adds no factors and is the smallest zero-free digit.

# Algorithm

1. Factorize `t` into counts of `2,3,5,7`. If anything remains, return `-1`.
2. Scan `num` and count its prime factors. Also find the first `0`.
3. If `num` is zero-free and already has enough factors, return it.
4. Build prefix factor-count arrays so we can quickly know how many factors exist before position `i`.
5. Starting from the right, try changing position `i` to every larger digit `d`.
6. Calculate the factors still needed: `required - prefix factors - factors(d)`.
7. Use `buildDigits()` to create the smallest suffix containing those factors.
8. If the suffix fits, fill unused positions with `1` and return the result.
9. If no same-length answer exists, build the smallest valid number with length greater than `num`.
## buildDigits()

It compresses factors:

- `2³ → 8`
- `3² → 9`
- `2² → 4`
- `2×3 → 6`
- `2²×3 → 2,6`  
    Then it sorts the digits so the suffix is as small as possible.

# CODE (Mine)

```cpp
class Solution {
public:
    string smallestNumber(string num, long long t) {
        long long temp_t = t;
        int req_c2 = 0, req_c3 = 0, req_c5 = 0, req_c7 = 0;
        
        // Extract the required prime factors from t
        while (temp_t % 2 == 0) { req_c2++; temp_t /= 2; }
        while (temp_t % 3 == 0) { req_c3++; temp_t /= 3; }
        while (temp_t % 5 == 0) { req_c5++; temp_t /= 5; }
        while (temp_t % 7 == 0) { req_c7++; temp_t /= 7; }
        
        // If t has prime factors other than 2, 3, 5, or 7, it's impossible to form with digits 1-9
        if (temp_t > 1) return "-1";
        
        int n = num.length();
        
        // Check if `num` itself is already valid (Zero-free + meets factor requirements)
        bool num_valid = true;
        int have_c2_total = 0, have_c3_total = 0, have_c5_total = 0, have_c7_total = 0;
        int zero_idx = n; // Tracks the first occurrence of '0'
        
        for (int i = 0; i < n; i++) {
            if (num[i] == '0') {
                num_valid = false;
                if (zero_idx == n) zero_idx = i;
            } else {
                int d = num[i] - '0';
                int temp = d;
                while (temp % 2 == 0) { have_c2_total++; temp /= 2; }
                while (temp % 3 == 0) { have_c3_total++; temp /= 3; }
                while (temp % 5 == 0) { have_c5_total++; temp /= 5; }
                while (temp % 7 == 0) { have_c7_total++; temp /= 7; }
            }
        }
        
        if (num_valid && have_c2_total >= req_c2 && have_c3_total >= req_c3 && 
            have_c5_total >= req_c5 && have_c7_total >= req_c7) {
            return num;
        }
        
        // Precompute prefix factors to quickly check what we've accumulated up to index `i`
        vector<int> pref_c2(n + 1, 0), pref_c3(n + 1, 0), pref_c5(n + 1, 0), pref_c7(n + 1, 0);
        for (int i = 0; i < n; i++) {
            pref_c2[i+1] = pref_c2[i]; pref_c3[i+1] = pref_c3[i];
            pref_c5[i+1] = pref_c5[i]; pref_c7[i+1] = pref_c7[i];
            
            if (num[i] > '0') {
                int d = num[i] - '0';
                int temp = d;
                while (temp % 2 == 0) { pref_c2[i+1]++; temp /= 2; }
                while (temp % 3 == 0) { pref_c3[i+1]++; temp /= 3; }
                while (temp % 5 == 0) { pref_c5[i+1]++; temp /= 5; }
                while (temp % 7 == 0) { pref_c7[i+1]++; temp /= 7; }
            }
        }
        
        auto getFactors = [](int d, int& c2, int& c3, int& c5, int& c7) {
            c2 = c3 = c5 = c7 = 0;
            int temp = d;
            while (temp % 2 == 0) { c2++; temp /= 2; }
            while (temp % 3 == 0) { c3++; temp /= 3; }
            while (temp % 5 == 0) { c5++; temp /= 5; }
            while (temp % 7 == 0) { c7++; temp /= 7; }
        };
        
        // Lambda to compute the lexicographically smallest digits for the remaining prime factors
        auto buildDigits = [](int c2, int c3, int c5, int c7) {
            string res = "";
            res += string(c7, '7');
            res += string(c5, '5');
            res += string(c3 / 2, '9'); c3 %= 2; // Pack 3s into 9s
            res += string(c2 / 3, '8'); c2 %= 3; // Pack 2s into 8s
            
            // Handle cross remainders
            if (c3 == 0 && c2 == 1) res += "2";
            else if (c3 == 0 && c2 == 2) res += "4";
            else if (c3 == 1 && c2 == 0) res += "3";
            else if (c3 == 1 && c2 == 1) res += "6";
            else if (c3 == 1 && c2 == 2) res += "26";
            
            sort(res.begin(), res.end());
            return res;
        };
        
        // Iterate right to left. We can't keep a prefix that includes a '0', so cap `i` at `zero_idx`.
        for (int i = min(n - 1, zero_idx); i >= 0; i--) {
            // We must strictly increase the number, so d starts at num[i] + 1
            // If the digit is '0', starting at '1' satisfies the strict increase since '1' > '0'
            int start_d = (num[i] == '0' ? 1 : num[i] - '0' + 1);
            
            for (int d = start_d; d <= 9; d++) {
                int d_c2, d_c3, d_c5, d_c7;
                getFactors(d, d_c2, d_c3, d_c5, d_c7);
                
                // Calculate missing prime factors
                int rem_c2 = max(0, req_c2 - pref_c2[i] - d_c2);
                int rem_c3 = max(0, req_c3 - pref_c3[i] - d_c3);
                int rem_c5 = max(0, req_c5 - pref_c5[i] - d_c5);
                int rem_c7 = max(0, req_c7 - pref_c7[i] - d_c7);
                
                string rem_str = buildDigits(rem_c2, rem_c3, rem_c5, rem_c7);
                
                // If our required factors can fit into the remaining string length
                if (rem_str.length() <= n - 1 - i) {
                    string ans = num.substr(0, i) + to_string(d);
                    int pad = n - 1 - i - rem_str.length();
                    ans += string(pad, '1') + rem_str; // Pad with 1s to keep the number small
                    return ans;
                }
            }
        }
        
        // If no replacement of the same length is possible, we must add another digit to the string length
        string rem_str = buildDigits(req_c2, req_c3, req_c5, req_c7);
        int final_len = max(n + 1, (int)rem_str.length());
        string ans = string(final_len - rem_str.length(), '1') + rem_str;
        return ans;
    }
};
```
# Dry Run

`num = "1234", t = 256`

`256 = 2⁸`, so `req_c2 = 8`.

Factors in `1234`:  
`1 → 0`, `2 → 2`, `3 → 3`, `4 → 2²`.  
So `num` has only three `2`s, therefore it isn't valid.

Now try changing digits from right to left.

At index `3` (`4`), try `5...9`. None can provide the remaining factors because there are no positions left.

At index `2` (`3`), try `4...9`. Still not enough space for the required factors.

At index `1` (`2`), try `3...9`.  
When `d = 4`:

- Prefix `"1"` gives `0` twos.
- `4` gives `2` twos.
- Need `8 - 0 - 2 = 6` more twos.
- `6` twos can be represented as `8,8`.
- There are exactly 2 positions left.

So:  
`prefix + d + suffix = "1" + "4" + "88" = "1488"`

Product:  
`1×4×8×8 = 256`, so the answer is `"1488"`.

# Time & Space Complexity

Let `n = num.length()`.

**Time:** `O(n + log t)`.  
We scan `num` a few times, and the greedy search checks at most 9 digits per position, which is still `O(n)`.

**Space:** `O(n)`.  
The four prefix arrays each have size `n+1`.
# Key Idea

`Factorize t → count factors → greedily increase one digit from right to left → build the smallest valid suffix.`  