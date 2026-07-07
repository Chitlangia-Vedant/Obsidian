# [42. Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/)

Given `n` non-negative integers representing an elevation map where the width of each bar is `1`, compute how much water it can trap after raining.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/rainwatertrap.png)

**Input:** height = [0,1,0,2,1,0,1,3,2,1,2,1]
**Output:** 6
**Explanation:** The above elevation map (black section) is represented by array [0,1,0,2,1,0,1,3,2,1,2,1]. In this case, 6 units of rain water (blue section) are being trapped.

**Example 2:**

**Input:** height = [4,2,0,3,2,5]
**Output:** 9

**Constraints:**

- `n == height.length`
- `1 <= n <= 2 * 104`
- `0 <= height[i] <= 105`

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        
    }
};
```

# Solution 1 
1. Find the maximum height and it's index. `m`
2. Iterate through the array `height` 
	1. From start to maximum height
	2. And end to maximum height
	3.  Store the maximum height seen `mx`
	4. If the current height is smaller than `mx` 
		- Increase the `count`
	5. If the current height is greater than `mx`
		- Store it in `mx`
3.  Return `count`

## CODE (Mine)

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        
        pair<int,int> m={-1,-1};
        for(int i=0;i<height.size();i++){
            if(m.first<=height[i]){
                m.first=height[i];
                m.second=i;
            }
        }
        int count=0;
        int mx=0;
        for(int i=0;i<m.second;i++){
            if(height[i]<mx){
                count+=(mx-height[i]);
            }else{
                mx=height[i];
            }
        }
        mx=0;
        for(int i=height.size()-1;i>m.second;i--){
            if(height[i]<mx){
                count+=(mx-height[i]);
            }else{
                mx=height[i];
            }
        }
        return count;
    }
};
```

# Solution 2

## Approach

To keep water, we have to have a bar on the left side and on the right side. Between them, we can keep water.

Example.

```
Input: height = [2,1,0,1,3,2]
```

![スクリーンショット 2024-05-08 1.03.27.png](https://assets.leetcode.com/users/images/5a1d20ee-1685-4eb5-98a9-b34596762c26_1715097837.7286208.png)

I think we can easily imagine that we can keep water at `0 height`(= index `2`), because we have bars in the adjacent places(= index `1` and `3`).

But how about `1 height` at index `1`. We can keep water there. I think left side is easy to imagine because we have a bar in adjacent place(= index `0`). But how about right side? In the end, we can keep water because we have `3 height` at index `4`.

---
## How it works

```python
[2,1,0,1,3,2]
 L         R

left max = 2
right max = 2
water = 0
```

`L` is current left pointer.  
`R` is current right pointer.  
`left max` is max height of left side we found so far. Initialized with the first number.  
`right max` is max height of right side we found so far. Initialized with the last number  
`water` is return value.

1. If `L` is smaller than `R`, we continue. 
2. After that, check `left max` and `right max` and **take smaller max height**.
	-  In this case, they are the same, so we can choose one of them.(`right max`)
3. Move `R` to the next.

```python
[2,1,0,1,3,2]
 L       R

left max = 2
right max = 2
water = 0
```

4. Update `right max` if current bar is taller than current `right max`.

```sql
current bar vs current max right
= 3 vs 2
= 3

[2,1,0,1,3,2]
 L       R

left max = 2
right max = 3
water = 0
```

5. Count number of water. Formula is

```sql
water = current right max - current bar
= 3 - 3
= 0
```

`water` should be `0`.

1.  `L < R`, so we continue
2. Take smaller max height between left and right. (`left max`).
3. Move `L` to the next and update `left max` if needed. No update this time.

```python
[2,1,0,1,3,2]
   L     R

left max = 2
right max = 3
water = 0
```

5. Then here is an important point. `water` should be

```sql
current left max(Since, left max<right max) - current bar
2 - 1 = 1

water = 1
```

---

```python
[2,1,0,1,3,2]
   L     R

left max = 2
right max = 3
water = 1
```

1. `L < R`, so we continue.  
2. Move `L` to the next because left max is smaller than right max and update `left max` if needed. No update this time.

```python
[2,1,0,1,3,2]
     L   R

left max = 2
right max = 3
water = 1
```

5. `water` should be...

```java
2 - 0 = 2
total water = 3
```

1. `L < R`, so we continue.  
2. Move `L` to the next because left max is smaller than right max and update `left max` if needed. No update this time.

```python
[2,1,0,1,3,2]
       L R

left max = 2
right max = 3
water = 3
```

5. `water` should be...

```java
2 - 1 = 1
total water = 4
```

1. `L < R`, so we continue.  
2. Move `L` to the next because left max is smaller than right max and update `left max` if needed. we found `3` this time.

```python
[2,1,0,1,3,2]
         L
         R
left max = 3 (updated. 2 vs 3)
right max = 3
water = 3
```

5. `water` should be...

```java
3 - 3 = 0
total water = 4
```

1. Now `L == R`. We stop iteration.

```kotlin
return 4
```

---
## Complexity

- Time complexity: O(n)
- Space complexity: O(1)

## CODE

```python
class Solution:
    def trap(self, height: List[int]) -> int:
        left = 0
        right = len(height) - 1
        left_max = height[left]
        right_max = height[right]
        water = 0

        while left < right:
            if left_max < right_max:
                left += 1
                left_max = max(left_max, height[left])
                water += left_max - height[left]
            else:
                right -= 1
                right_max = max(right_max, height[right])
                water += right_max - height[right]
        
        return water
```

## Step by Step Algorithm

1. **Initialize pointers and variables**:
    - Initialize two pointers `left` and `right` at the beginning and end of the `height` list respectively.
    - Initialize variables `left_max` and `right_max` to store the maximum height encountered from the left and right sides respectively.
    - Initialize a variable `water` to keep track of the total trapped water.

```python
left = 0
right = len(height) - 1
left_max = height[left]
right_max = height[right]
water = 0
```

2. **Loop until pointers meet**:
    - Continue looping while `left` pointer is less than `right` pointer, indicating there are still bars to process.

```python
while left < right:
```

3. **Check which side to move**:
    - Compare `left_max` and `right_max` heights.
    - If `left_max` is less than `right_max`, move the `left` pointer to the right and update `left_max`.
    - Otherwise, move the `right` pointer to the left and update `right_max`.

```python
if left_max < right_max:
    left += 1
    left_max = max(left_max, height[left])
    water += left_max - height[left]
else:
    right -= 1
    right_max = max(right_max, height[right])
    water += right_max - height[right]
```

4. **Calculate trapped water**:
    
    - Calculate the water trapped at the current position based on the difference between the maximum height and the current height.
    - Accumulate this water amount to the `water` variable.
5. **Return total trapped water**:
    
    - After the loop ends, return the total trapped water accumulated in the `water` variable.

```python
return water
```
