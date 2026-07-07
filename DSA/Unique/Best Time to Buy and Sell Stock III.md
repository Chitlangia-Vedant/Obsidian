# [123. Best Time to Buy and Sell Stock III](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete **at most two transactions**.

**Note:** You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

**Example 1:**

**Input:** prices = [3,3,5,0,0,3,1,4]
**Output:** 6
**Explanation:** Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

**Example 2:**

**Input:** prices = [1,2,3,4,5]
**Output:** 4
**Explanation:** Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.

**Example 3:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transaction is done, i.e. max profit = 0.

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 105`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        
    }
};
```

# Solution 1

## CODE(Mine)

```cpp
class Solution {
public:
    struct prof{
            int profit;
            int min;
            int max;
    };
    vector<prof> dp1;
    vector<prof> dp2;
    vector<int> price;
    int size;
    int maxProfit(const vector<int>& prices) {
        
        dp1.assign(prices.size(),{-1,-1,-1});
        dp2.assign(prices.size(),{-1,-1,-1});
        price=prices;
        size=price.size();
        int mx=0; 
        for(int i=0;i<size;i++){
            mx=max(mx,profit1(i).profit+profit2(i).profit);
        }
        for(int i=0;i<size;i++){
            cout<<dp1[i].profit<<" "<<dp2[i].profit<<"\n";
        }
        return mx;
    }
    prof profit1(int i) {
        if(dp1[i].profit!=-1) return dp1[i];
        if(i==0){
            dp1[i]={0,price[i],price[i]};
            return dp1[i];
        }
        prof prev=profit1(i-1);
        if(price[i]>prev.max){
            dp1[i]={price[i]-prev.min,prev.min,price[i]};
            return dp1[i];
        }
        if(price[i]<prev.min){
            dp1[i]={prev.profit,price[i],prev.max};
            return dp1[i];
        }
        if(price[i]-prev.min>prev.profit){
            dp1[i]={price[i]-prev.min,prev.min,price[i]};
            return dp1[i];
        }
        dp1[i]=prev;
        return dp1[i];
    }
    prof profit2(int i){
        if(dp2[i].profit!=-1) return dp2[i];
        if(i==size-1){
            dp2[i]={0,price[i],price[i]};
            return dp2[i];
        }
        prof prev=profit2(i+1);
        if(price[i]>prev.max){
            dp2[i]={prev.profit,prev.min,price[i]};
            return dp2[i];
        }
        if(price[i]<prev.min){
            dp2[i]={prev.max-price[i],price[i],prev.max};
            return dp2[i];
        }
        if(prev.max-price[i]>prev.profit){
            dp2[i]={prev.max-price[i],price[i],prev.max};
            return dp2[i];
        }
        dp2[i]=prev;
        return dp2[i];
    }
};
```

# Solution 2

### Thought Process

1. **Single transaction is simple**
    
    - Buy at minimum price, sell at maximum price
    - Need 2 variables: `min_price` and `max_profit`
2. **Two transactions become complex**
    
    - When to complete the first transaction?
    - When to start the second transaction?
    - These decisions must be made daily
3. **Think in terms of states**
    
    - Optimal decisions depend on current "state"
    - 4 states: "after 1st buy", "after 1st sell", "after 2nd buy", "after 2nd sell"

## Meaning of Four Variables

|Variable|State|Meaning|
|---|---|---|
|`buy1`|After 1st buy|Maximum profit while holding first stock|
|`sell1`|After 1st sell|Maximum profit after completing first transaction|
|`buy2`|After 2nd buy|Maximum profit while holding second stock|
|`sell2`|After 2nd sell|Maximum profit after completing both transactions|

**Key Point**: Each variable maintains the "maximum profit in that state"

## Why Check Daily?

Every day, we have choices:

- **Do nothing**: Maintain current state
- **Take action**: Buy or sell

To find the optimal solution, we must decide daily whether to "act or not act."

## How it works

```
prices = [1,5,2,4]
```

### Initialization (day1: price = 1)

We initialize 4 variables. buy actions will be initialized with price on day 1. But these are buy action, so we initialize `-price`.

We can't sell stocks on the first day, sell action will be initialized with 0.

```python
buy1 = -1   # Profit after buying at price 1 = -1
sell1 = 0   # Haven't sold yet
buy2 = -1   # If we buy second stock at price 1 = -1  
sell2 = 0   # Haven't sold second stock yet
```

We start iteration from day2(= index 1).

---

### Day2: price = 5

**Consider choices for each state:**

1. **Update buy1**  
    buy1 is the first buy, so we comapre current buy1 with minus today's price. If today is day5, we compare
    
    ```lisp
    the best buy(= maxinum profit) between day1 and day4
    vs
    today's buy action(minus today's price)
    ```
    
    ```python
    buy1 = max(buy1, -price)
         = max(-1, -5)
         = -1
    ```
    
    - Already bought at price 1, no benefit buying at price 5
2. **Update sell1**  
    We use buy1 becuase buy1 is the maximum profit for buy action when we bought a stock between day1 and today. There is a possiblity that we will get maximum profit if we add today's price to buy1.
    
    ```python
    sell1 = max(sell1, buy1 + price)
          = max(0, -1 + 5)
          = 4
    ```
    
    - Selling stock bought at price 1 for price 5 gives profit 4!
3. **Update buy2**  
    buy2 will happen after sell1, so we use sell1 to get maximum profit when we do buy2, so buy2 is maximum profit after completing one buy-sell transaction + second buy action
    
    ```python
    buy2 = max(buy2, sell1 - price)
         = max(-1, 4 - 5)
         = -1
    ```
    
    - Using first transaction profit (4) to buy at price 5 is worse than keeping -1
4. **Update sell2**  
    sell2 will happen after buy1 which means sell2 is the final maximum profit and return value. We add today's price to buy2.
    
    ```python
    sell2 = max(sell2, buy2 + price)
          = max(0, -1 + 5)
          = 4
    ```
    
    - Selling second stock also gives profit 4

**End of Day 1:**

```
buy1 = -1, sell1 = 4, buy2 = -1, sell2 = 4
```

---

I'll speed up.

### Day3: price = 2

1. **Update buy1**
    
    ```python
    buy1 = max(-1, -2) = -1
    ```
    
    - Still better to have bought at price 1
2. **Update sell1**
    
    ```python
    sell1 = max(4, -1 + 2) = 4
    ```
    
    - Better to keep previous profit of 4 than sell at price 2
3. **Update buy2**
    
    ```python
    buy2 = max(-1, 4 - 2) = 2
    ```
    
    - **Critical insight!** Use first transaction profit (4) to buy at price 2, net profit = 2
4. **Update sell2**
    
    ```python
    sell2 = max(4, 2 + 2) = 4
    ```
    
    - Both strategies give same profit

**End of Day 2:**

```
buy1 = -1, sell1 = 4, buy2 = 2, sell2 = 4
```

We finally update buy2. That means we may start the second transaction!

---

### Day4: price = 4

1. **Update buy1**
    
    ```python
    buy1 = max(-1, -4) = -1
    ```
    
2. **Update sell1**
    
    ```python
    sell1 = max(4, -1 + 4) = 4
    ```
    
3. **Update buy2**
    
    ```python
    buy2 = max(2, 4 - 4) = 2
    ```
    
4. **Update sell2**
    
    ```python
    sell2 = max(4, 2 + 4) = 6
    ```
    
    - **Final answer!** Sell second stock (bought at effective price 2) at price 4 for total profit 6

**Final Result:**

```
buy1 = -1, sell1 = 4, buy2 = 2, sell2 = 6
```

```kotlin
return 6(= sell2)
```

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buy1 = -prices[0];
        int sell1 = 0;
        int buy2 = -prices[0];
        int sell2 = 0;

        for (int i = 1; i < prices.size(); i++) {
            int price = prices[i];
            buy1 = max(buy1, -price);
            sell1 = max(sell1, buy1 + price);
            buy2 = max(buy2, sell1 - price);
            sell2 = max(sell2, buy2 + price);
        }

        return sell2;        
    }
};
```