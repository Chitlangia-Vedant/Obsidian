#  [875. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/)

Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

**Example 1:**

**Input:** piles = [3,6,7,11], h = 8
**Output:** 4

**Example 2:**

**Input:** piles = [30,11,23,4,20], h = 5
**Output:** 30

**Example 3:**

**Input:** piles = [30,11,23,4,20], h = 6
**Output:** 23

**Constraints:**

- `1 <= piles.length <= 104`
- `piles.length <= h <= 109`
- `1 <= piles[i] <= 109`

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        
    }
};
```

# Solution TLE

## CODE (Mine)

```cpp
class Solution {
public:
    int minEatingSpeed(const vector<int>& piles, int h) {
    // Your logic goes here
    auto cmp = [](const std::pair<int, int>& a, const std::pair<int, int>& b) {
        return static_cast<double>(a.first/ a.second) < static_cast<double>(b.first / b.second);
    };

    // Declare the priority_queue using decltype and pass the lambda to the constructor
    std::priority_queue<
        std::pair<int, int>, 
        std::vector<std::pair<int, int>>, 
        decltype(cmp)
    > pq(cmp);    
    for(int bananas:piles){
        h--;
        pq.push({bananas,1});
    }
    int k=((pq.top().first+pq.top().second-1)/pq.top().second);
    while(h>0){
        pq.push({pq.top().first,pq.top().second+1});
        pq.pop();
        k=((pq.top().first+pq.top().second-1)/pq.top().second);
        h--;
    }
    return k;
}
};
```

# Solution 2

As we know that the potential rate that we're eating bananas at **k** is going to be between **1** that's the minimum it could possibly be. The max it could possibly be is going to be whatever the max in our input array is and that is **11**.  
So, then we're going to initialize a range like this **k = [1,2,3,4,5,6,7,8,9,10,11]** the entire range we have. Going all the way from **1** to the max value **11**.  
![image](https://assets.leetcode.com/users/images/012f952e-b395-419a-b8fc-cfafd16cb907_1642649929.7762113.png)

So, in other words we're going to have a **left pointer** at the **minimum** and a **right pointer** at the **maximum** . Then we compute the middle by taking the **average of left & right / 2 i.e. 1 + 11 / 2 = 6**. So, our **middle** will be here at **6** in other words that **k** we're trying is going to be here at this rate that we're going to eat bananas at rate of **6** .

`Now let's check can we eat all the piles of bananas at rate of 6. Let's check it,`  
![image](https://assets.leetcode.com/users/images/e89c158c-d94e-470d-b614-5cce7c826784_1642650601.5053208.png)

If you see that we just eat all piles of bananas in **6 hour** is that a good value. Well it is less than or equals to **8 hours**, but still we have to find the **minimum possible** of **k** value. This might be the solution but less try is there any more **smaller** **k** value then **6**.  
So, we **decrement** our **right pointer to mid - 1** becuase there might be the best possible solution available.

So, once again we compute the middle by taking the **average of left & right / 2 i.e. 1 + 5 / 2 = 3** our **k** is here at **3** value

`Now let's check can we eat all the piles of bananas at rate of 3. Let's check it,`  
![image](https://assets.leetcode.com/users/images/6d2a2019-34c4-49f7-b5ad-f4468862d490_1642651233.9354265.png)

If you see that we just eat all piles of bananas in **10 hour** but if you see we went over **8 hours** we took too long to eat all the bananas. So, eating at **rate of 3** didn't work.  
Let's start searching to right of our range we **increment** our **left pointer to mid + 1** _but remember when we shift our right pointer from the last position to mid -1 we just consider that this range will not work. And that's how Binary search work!_

So, once again we compute the middle by taking the **average of left & right / 2 i.e. 4 + 5 / 2 = 4** our **k** is here at **4** value

`Now let's check can we eat all the piles of bananas at rate of 4. Let's check it,`  
![image](https://assets.leetcode.com/users/images/ee337cb4-121f-4239-93f0-825e66119386_1642653637.250936.png)

If you see that we just eat all piles of bananas in **8 hour**. So, we able to eat all bananas in less than or equals to **8 hours** if we had a rate of **4**. Let's compare this with our current result. So, far we find a value of **6** we update this **6** and we can say there is a more smaller rate we can use i.e. **4**.

## CODE

```cpp
class Solution {
public:
    int minEatingSpeed(vector<int>& piles, int h) {
        int left = 1;
        int right = 1000000000;
        
        while(left <= right){
            int mid = left + (right - left) / 2;
            if(canEatInTime(piles, mid, h)) right = mid - 1;
            else left = mid + 1;
        }
        return left;
    }
    bool canEatInTime(vector<int>& piles, int k, int h){
        int hours = 0;
        for(int pile : piles){
            int div = pile / k;
            hours += div;
            if(pile % k != 0) hours++;
        }
        return hours <= h;
    }
};
```

- **Time Complexity :-** O(N * log(M)) where N is no of piles & M is range of K (left to right)
- **Space Complexity :-** O(1) as not using any extra space