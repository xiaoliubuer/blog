---
layout: post
title:  "121. Best Time to Buy and Sell Stocke"
date: 2019-05-12 14:28:00 -0400
categories: articles
---

Say you have an array for which the ith element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:
```
Input: [7,1,5,3,6,4]
Output: 5
```
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.
Example 2:
```
Input: [7,6,4,3,1]
Output: 0
```
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int T_i10 = 0, T_i11 = INT_MIN;
        for ( int price : prices ) {
            T_i10 = max(T_i10, T_i11 + price);
            T_i11 = max(T_i11, -price);
        }
        return T_i10;
    }
};
```
```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.size() == 0) return 0;
        int res = 0;
        for (int buy = 0, sale = 1; sale < prices.size(); sale++) {
            if ( prices[buy] >= prices[sale]) buy = sale;
            else {
                res = max(res, prices[sale] - prices[buy]);
            }
        }
        return res;
    }
};
```