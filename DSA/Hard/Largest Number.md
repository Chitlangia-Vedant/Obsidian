# [179. Largest Number](https://leetcode.com/problems/largest-number/)

Given a list of non-negative integers `nums`, arrange them such that they form the largest number and return it.

Since the result may be very large, so you need to return a string instead of an integer.

**Example 1:**

**Input:** nums = [10,2]
**Output:** "210"

**Example 2:**

**Input:** nums = [3,30,34,5,9]
**Output:** "9534330"

**Constraints:**

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 109`

# Understanding:

Different Numbers: Shuffling Allowed
[10,2] -> "210", "102": Correct

Within Same Number: Shuffling Digits is NOT Allowed
[10,2] -> "201", "120": Incorrect

```
[3,30] -> 
"330": Largest
"303"
```

# Solution:

Comparator Based Sorting:

String s1 = "3"
String s2 = "30";

String case1 = s2 + s1 = "303";

String case2 = s1 + s2 = "330"; ------ ANSWER

case2 > case1

# Approach:

(1) Convert All Numbers/Integers to String
(2) Sort in Descending Order as per Criteria: Comparator Based Sorting
(3) Append in Solution

Edge Case: Handle ALL Values as 0 in the Array NOT just having 1 or more non-zero values in the Array

Input: nums = [9,0,6,5]
OP: "9650": Already Handled in Decreasing Order

Input: nums = [0,0,0,1]
OP: "1000": Already Handled in Decreasing Order

(4) Special handling: Array with All 0
- Delete All 0 Except 1

Input: nums = [0,0,0,0]
OP: "0000": Incorrect OP

After Handling, 
Correct OP: "0"

NOTE:

After Step-2, Comparator Based Sorting:
First Character is '0', Then All Values are 0 in the Array


00001
"10000"

012012
"221100"

# Approach

1. Convert Integers to Strings: Since comparing individual digits of integers won't work directly for concatenation, first convert the integer array into an array of strings. This allows us to concatenate and compare the results of a + b and b + a.
2. Custom Sorting: Use a custom comparator to sort the string array. The comparator should compare two strings a and b by checking the results of a + b vs b + a. If a + b is larger, then a should come before b in the sorted array.
3. Edge Case: After sorting, if the largest number is "0" (i.e., if the first element in the sorted array is "0"), return "0", since this means all numbers were zeros.
4. Construct the Result: Use a StringBuilder to concatenate all the strings in the sorted array into the final result.

# CODE:

```Java
class Solution {
    public String largestNumber(int[] nums) 
    {
        if (nums == null || nums.length == 0)
            return "";
        
        int i = 0;
        int n = nums.length;
        String[] s_num = new String[n];

        for (i=0; i<n; i++)
        {
            s_num[i] = String.valueOf(nums[i]);
            // toString() in C++
        }

        Comparator<String> comp = new Comparator<String>()
        {
            @Override
            public int compare(String s1, String s2)
            {
                String str1 = s1 + s2;
                String str2 = s2 + s1;

                //System.out.println(str2.compareTo(str1));
                //System.out.println(str1.compareTo(str2));
                return str2.compareTo(str1); // REVERSE

                // s1 > s2: s1.compareTo(s2): 1
                // s1 < s2: s1.compareTo(s2): -1

/*
            s1 = 1, s2 = 2
            12.compareTo(21) = -1  ----> "21"

            s1 = 3, s2 = 2
            32.compareTo(23) = 1 -----: "32"
*/
            }
        };

        Arrays.sort(s_num, comp); // O(NlogN)

        // if AFTER Comparator Based Sorting, First Character is '0', The All Values are 0 in the Array
        // Edge Case: [0 0 0 0]
        if (s_num[0].charAt(0) == '0') 
            return "0";
        
        StringBuilder ans = new StringBuilder();
        for (String s: s_num)
            ans.append(s);

        return ans.toString();       
    }
}
```

k: Average length of string in s_num[]
TC: O(NlogN * k) + O(N) = O(k * NlogN)
SC: O(N)

# CODE(C++)

```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        // Convert integers to strings
        vector<string> array;
        for (int num : nums) {
            array.push_back(to_string(num));
        }

        // Custom comparator for sorting
        sort(array.begin(), array.end(), [](const string &a, const string &b) {
            return (b + a) < (a + b);
        });

        // Handle the case where the largest number is "0"
        if (array[0] == "0") {
            return "0";
        }

        // Build the largest number from the sorted array
        string largest;
        for (const string &num : array) {
            largest += num;
        }

        return largest;
    }
};
```

