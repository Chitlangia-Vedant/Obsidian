# [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return _the maximum profit you can achieve from this transaction_. If you cannot achieve any profit, return `0`.

**Example 1:**

**Input:** prices = [7,1,5,3,6,4]
**Output:** 5
**Explanation:** Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.

**Example 2:**

**Input:** prices = [7,6,4,3,1]
**Output:** 0
**Explanation:** In this case, no transactions are done and the max profit = 0.

**Constraints:**

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        
    }
};
```

# Solution 1

1. Iterate the array in reverse.
2. Store the max value `m` as we go through the array,
3. Calculate the max profit for each day. 
	- max value-day value
	- `m` - `i`
4. Store the overall max profit `m_profit` 
	 - Compare it with the max profit for each day.
	 - `max(m_profit,m-1)`
5. Overall max profit `m_profit` after the full iteration is the answer
## CODE (Mine)

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int m=-1;
        int m_profit=0;
        for(auto &i:views::reverse(prices)){
            m=max(m,i);
            m_profit=max(m_profit,m-i);
        }
        return m_profit;
    }
};
```

# Solution 2: Kadane Algo.

## Approach

Simply, when we find low price of stock, we should buy it so that we can get maximum profit in a later day.

```
Input: prices = [7,1,5,3,6,4]
```

Initialize

```python
buy_price = 7
profit = 0

7 is the first price in input array
```

We start from index `1`.

```java
[7,1,5,3,6,4]
   p

buy_price = 7
profit = 0
```

We find `price 1`. Let's think about whether we should update `buy_price` or not.

As I told you, we should buy the stock when we find low price because there is possiblity that we can get maximum profit in a later day.

Calculation of profit is very simple

```sql
profit = current price - buy price
```

At day 1, we find `price 1`, so we should buy it at day 1. Update `buy_price` with `price 1`

```
buy_price = 1
```

Then calculate profit.

```sql
profit = max(profit, current price - buy price)
0 = max(0, 1 - 1)
```

Next, I'll speed up.

```java
[7,1,5,3,6,4]
     p

buy_price = 1
profit = 0
```

Update `buy_price`? → No, because previous `buy_price` is lower than current price.

Calculate profit.

```python
profit = max(0, 5 - 1)
= 4
```

Next,

```java
[7,1,5,3,6,4]
       p

buy_price = 1
profit = 4
```

Update `buy_price`? → No  
Calculate profit.

```python
profit = max(4, 3 - 1)
= 4
```

Next,

```java
[7,1,5,3,6,4]
         p

buy_price = 1
profit = 4
```

Update `buy_price`? → No  
Calculate profit.

```python
profit = max(4, 6 - 1)
= 5
```

Next,

```java
[7,1,5,3,6,4]
           p

buy_price = 1
profit = 5
```

Update `buy_price`? → No  
Calculate profit.

```python
profit = max(5, 4 - 1)
= 5
```

Finish iteration.

```kotlin
return 5
```

## Approach

Simply, when we find low price of stock, we should buy it so that we can get maximum profit in a later day.

```
Input: prices = [7,1,5,3,6,4]
```

Initialize

```python
buy_price = 7
profit = 0

7 is the first price in input array
```

We start from index `1`.

```java
[7,1,5,3,6,4]
   p

buy_price = 7
profit = 0
```

We find `price 1`. Let's think about whether we should update `buy_price` or not.

As I told you, we should buy the stock when we find low price because there is possiblity that we can get maximum profit in a later day.

Caculation of profit is very simple

```sql
profit = current price - buy price
```

At day 1, we find `price 1`, so we should buy it at day 1. Update `buy_price` with `price 1`

```
buy_price = 1
```

Then calclate profit.

```sql
profit = max(profit, current price - buy price)
0 = max(0, 1 - 1)
```

Next, I'll speed up.

```java
[7,1,5,3,6,4]
     p

buy_price = 1
profit = 0
```

Update `buy_price`? → No, because previous `buy_price` is lower than current price.

Calculate profit.

```python
profit = max(0, 5 - 1)
= 4
```

Next,

```java
[7,1,5,3,6,4]
       p

buy_price = 1
profit = 4
```

Update `buy_price`? → No  
Calculate profit.

```python
profit = max(4, 3 - 1)
= 4
```

Next,

```java
[7,1,5,3,6,4]
         p

buy_price = 1
profit = 4
```

Update `buy_price`? → No  
Calculate profit.

```python
profit = max(4, 6 - 1)
= 5
```

Next,

```java
[7,1,5,3,6,4]
           p

buy_price = 1
profit = 5
```

Update `buy_price`? → No  
Calculate profit.

```python
profit = max(5, 4 - 1)
= 5
```

Finish iteration.

```kotlin
return 5
```

## CODE

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int buyPrice = prices[0];
        int profit = 0;

        for (int i = 1; i < prices.size(); i++) {
            if (buyPrice > prices[i]) {
                buyPrice = prices[i];
            }

            profit = max(profit, prices[i] - buyPrice);
        }

        return profit;        
    }
};
```

## Step by Step Algorithm

1. **Initialize Variables**:  
    Initialize `buy_price` to the first price in the list and `profit` to 0.

```python
buy_price = prices[0]
profit = 0
```

2. **Iterate Over Prices**  
    Loop through each price starting from the second price (`prices[1:]`).

```python
for p in prices[1:]:
```

3. **Update Buy Price**  
    If the current price (`p`) is less than the previously set `buy_price`, update `buy_price` to the current price. This ensures that we buy at the lowest price possible.

```python
if buy_price > p:
    buy_price = p
```

4. **Calculate Profit**  
    Calculate the profit by subtracting the `buy_price` from the current price (`p`). Update `profit` to the maximum value between the current profit and the calculated profit. This ensures we keep track of the maximum profit throughout the iteration.

```python
profit = max(profit, p - buy_price)
```

5. **Return Maximum Profit**  
    After the loop, return the final value of `profit`, which represents the maximum profit achievable from buying and selling stocks.

```python
return profit
```