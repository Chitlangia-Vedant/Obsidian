[135. Candy](https://leetcode.com/problems/candy/)

There are `n` children standing in a line.

Each child is assigned a rating value given in the integer array `ratings`.

You are giving candies to these children subjected to the following requirements:

- Each child must have **at least** one candy.
- Children with a **higher** rating get more candies than their neighbors.

Return the **minimum** number of candies you need to have to distribute the candies to the children.

**Example 1:**

**Input:** ratings = [1,0,2]
**Output:** 5
**Explanation:** You can allocate to the first, second and third child with 2, 1, 2 candies respectively.

**Example 2:**

**Input:** ratings = [1,2,2]
**Output:** 4
**Explanation:** You can allocate to the first, second and third child with 1, 2, 1 candies respectively.
The third child gets 1 candy because it satisfies the above two conditions.

**Constraints:**

- `1 <= n == ratings.length <= 5 * 104`
- `0 <= ratings[i] <= 5 * 104`

# Approach

## ■ Step 1

Intialize `total_candies` with `len(ratings)` because at least all children has one candy.

## ■ Step 2

We iterate through `ratings` one by one.

### case1

```
if ratings[i] == ratings[i - 1]:
```

we don't have to give a candy to the current `i` child because current `i` child has the same rating with previous child. Just increment `i` and continue.

### case2

```javascript
while i < n and ratings[i] > ratings[i - 1]:
```

This case indicates that the current `i` child has higher raiting than `i - 1` previous child. In this case, we need to give candies to the current `i` child.

Add `+1` to `current_peak` and add `current_peak` to `total_candies` and incremtnt `i`. this process continues until we don't meet case2 `while condition` above.

Let me explain `current_peak` in "How it works" section.

### case3

```javascript
while i < n and ratings[i] < ratings[i - 1]:
```

This case indicates that the current `i` child has lower raiting than `i - 1` previous child. In this case, actually we do the same thing of `case2`. Because even if input is decreasing order `[9,5,3,1]`, we need to give candies to the current `i` child. In the end, total candies are 10 `[4,3,2,1]`. we need some extra candies.

Add `+1` to `current_valley` and add `current_valley` to `total_candies` and incremtnt `i`. this process continues until we don't meet case 3 `while condition` above.

Let me explain `current_valley` in "How it works" section.

In the last of Step 2, subtract minium of `current_peak` or `current_valley` from `total_candies`.

# How it works

Let's think with this input.

```
Input: ratings = [2,4,6,7,3,2,2]
```

```lisp
total_candies(Let's say "total") = 7 (length of input array)
current_peak(Let's say "peak") = 0
current_valley(Let's say "valley") = 0
```

In Step2, starting from `index 1`.

```java
■ Case2
condtion: while i < n and ratings[i] > ratings[i - 1]:

index 1 > index 0, so peak = 1, valley = 0, total = 8 (7 + 1)
index 2 > index 1, so peak = 2, valley = 0, total = 10 (8 + 2)
index 3 > index 2, so peak = 3, valley = 0, total = 13 (10 + 3)
index 4 < index 3, then stop Case2

Our code distrubtes candies like this so far
from [1,1,1,1,1,1,1]
to   [1,2,3,4,1,1,1](total 13 candies)
```

```java
■ Case3
condtion: while i < n and ratings[i] < ratings[i - 1]:

index 4 < index 3, so peak = 3, valley = 1, total = 14 (13 + 1)
index 5 < index 4, so peak = 3, valley = 2, total = 16 (14 + 2)
index 6 == index 5, then stop Case3

Our code distrubtes candies like this so far
from [1,2,3,4,1,1,1](Case2)
to   [1,2,3,4,2,3,1](total 16 candies)
```

Let me explain `total_candies -= min(current_peak, current_valley)`  
Before that, `the last rating(index 6)` is equal to `index 5`, so we don't do anything(meet `Case1`). Let me skip it.

Now Look at this. When ratings = `[2,4,6,7,3,2,2]`

```csharp
This is real distribution, In other words, the minimum number of
candies you need to have to distribute

[1,2,3,4,2,1,1] = 14 candies
[1,1,1,1,1,1,1]
[0,1,2,3,1,0,0](additional candies for each children)
```

```csharp
How our code distributes candies

[1,2,3,4,2,3,1] = 16 candies
[1,1,1,1,1,1,1]
[0,1,2,3,1,2,0](additional candies for each children)
```

Compare distribution of adittional candies.

```csharp
[0,1,2,3,1,0,0](Real)
[0,1,2,3,1,2,0](Our code)
```

Our code distributes `extra 2 candies` at `index 5` which is equal to minimum of `current_peak` or `current_valley`. That's because actually we add peaks twice as a `peak` and as a `valley` in Case2 and Case3. For this question, we need peak value only once. More precisely, we need only higher peak between range of one of `peak` and `valley` combinations(in this case between `index1` and `index5`), because of constraints from the description saying "Children with a higher rating get more candies than their neighbors".

That's why we need to substract `minimum peak` from `total_candies`. In this case `16 - 2`. `peak = 3` vs `valley = 2`

```
Output: 14
```

# Complexity

- Time complexity: O(n)  
    'n' is the number of elements in the 'ratings' list. This is because we are using a single loop to iterate through the ratings, and within the loop, we perform constant time operations.
- Space complexity: O(1)
	which means it uses a constant amount of additional memory regardless of the size of the 'ratings' list. The only variables that consume memory are 'n', 'total_candies', 'i', 'current_peak', and 'current_valley', and these variables do not depend on the input size 'n'.

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
       int n = ratings.size();
        int totalCandies = n;
        int i = 1;

        while (i < n) {
            if (ratings[i] == ratings[i - 1]) {
                i++;
                continue;
            }

            int currentPeak = 0;
            while (i < n && ratings[i] > ratings[i - 1]) {
                currentPeak++;
                totalCandies += currentPeak;
                i++;
            }

            if (i == n) {
                return totalCandies;
            }

            int currentValley = 0;
            while (i < n && ratings[i] < ratings[i - 1]) {
                currentValley++;
                totalCandies += currentValley;
                i++;
            }

            totalCandies -= min(currentPeak, currentValley);
        }

        return totalCandies;        
    }
};
```