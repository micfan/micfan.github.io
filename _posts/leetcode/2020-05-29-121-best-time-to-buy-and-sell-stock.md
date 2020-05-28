---
title:  "121 - best time to buy and sell stock"
tags: 
    - Easy
    - Dynamic Programming
---
Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:
```
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
```

Example 2:
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

## Analysis

分析见 https://leetcode.com/articles/best-time-to-buy-and-sell-stock/

## Solution


### Time Limit Exceeded

```c++
/*
Approach 1: Brute Force
*/
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) return 0;
        int max_profit = 0;
        
        for (int i = 0; i < prices.size()-1; ++i) {
            for (int j = i+1; j < prices.size(); ++j) {
                int profit = prices[j] - prices[i];
                if ( profit > max_profit) {
                    max_profit = profit;   
                }
            }
        }
        
        return max_profit;
    }
};
```

```
Time Limit Exceeded
Details 

Last executed input
[10000,9999,9998,9997,9996,9995,9994,9993,9992,9991,9990,9989,9988,9987,9986,9985,9984,9983,9982,9981,9980,9979,9978,9977,9976,9975,9974,9973,9972,9971,9970,9969,9968,9967,9966,9965,...
View All
```

### 78.41%

```c++
// Approach 2: One Pass
// 不知道什么优化可以做到 100%
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() <= 1>) return 0;
        int min_price = INT_MAX; // std::numeric_limits<int>::max();
        int max_profit = 0;
        
        for (int i = 0; i < prices.size(); ++i) {
            int a = prices[i];

            if (a < min_price) {
                min_price = a;
            } else {
                int profit = a - min_price;
                if ( profit > max_profit) {
                    max_profit = profit;
                }   
            }
        }
        
        return max_profit;
    }
};
```

```
Success
Details 
Runtime: 8 ms, faster than 78.41% of C++ online submissions for Best Time to Buy and Sell Stock.
Memory Usage: 13.2 MB, less than 18.06% of C++ online submissions for Best Time to Buy and Sell Stock.
Next challenges:
Maximum Subarray
Best Time to Buy and Sell Stock II
Best Time to Buy and Sell Stock III
Best Time to Buy and Sell Stock IV
Best Time to Buy and Sell Stock with Cooldown
```