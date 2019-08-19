---
layout: post
title:  "638. Shopping Offers"
date: 2019-08-18 15:05:00 -0400
categories: articles
---	
In LeetCode Store, there are some kinds of items to sell. Each item has a price.

However, there are some special offers, and a special offer consists of one or more different kinds of items with a sale price.

You are given the each item's price, a set of special offers, and the number we need to buy for each item. The job is to output the lowest price you have to pay for exactly certain items as given, where you could make optimal use of the special offers.

Each special offer is represented in the form of an array, the last number represents the price you need to pay for this special offer, other numbers represents how many specific items you could get if you buy this offer.

You could use any of special offers as many times as you want.

Example 1:
```
Input: [2,5], [[3,0,5],[1,2,10]], [3,2]
Output: 14
Explanation: 
There are two kinds of items, A and B. Their prices are $2 and $5 respectively. 
In special offer 1, you can pay $5 for 3A and 0B
In special offer 2, you can pay $10 for 1A and 2B. 
You need to buy 3A and 2B, so you may pay $10 for 1A and 2B (special offer #2), and $4 for 2A.
```
Example 2:
```
Input: [2,3,4], [[1,1,0,4],[2,2,1,9]], [1,2,1]
Output: 11
Explanation: 
The price of A is $2, and $3 for B, $4 for C. 
You may pay $4 for 1A and 1B, and $9 for 2A ,2B and 1C. 
You need to buy 1A ,2B and 1C, so you may pay $4 for 1A and 1B (special offer #1), and $3 for 1B, $4 for 1C. 
You cannot add more items, though only $9 for 2A ,2B and 1C.
```
Note:
```
There are at most 6 kinds of items, 100 special offers.
For each item, you need to buy at most 6 of them.
You are not allowed to buy more items than you want, even if that would lower the overall price.
```

```c++
class Solution {
public:
    
    int directBuy(vector<int>& price, vector<int>& needs){
        int res = 0;
        for(int i = 0; i < needs.size(); i++){
            res += price[i]*needs[i];
        }
        return res;
    }
    
    int dfs(vector<int>& price, vector<vector<int>>& special, vector<int>& needs, int pos){
        int res = directBuy(price, needs);
        for(int i = pos; i < special.size(); i++){
            vector<int> tmp;
            int j = 0;
            for(; j < needs.size(); j++){
                if(needs[j] < special[i][j]){
                    break;
                }else{
                    needs[j] -= special[i][j];
                }
            }
            if(j == needs.size()){
                res = min(res, special[i][j] + dfs(price, special, needs, pos));
            }
            j--;
            for(; j >= 0; j--){
                needs[j] += special[i][j];
            }
        }
        return res;
    }
    
    int shoppingOffers(vector<int>& price, vector<vector<int>>& special, vector<int>& needs) {
        return dfs(price, special, needs, 0);
    }
};
```
```c++
class Solution {
public:
    int shoppingOffers(const vector<int>& price, const vector<vector<int>>& special, const vector<int>& needs) {
        int result = inner_product(price.begin(), price.end(), needs.begin(), 0);
        for (const vector<int>& offer : special) {
            vector<int> r = can(needs, offer);
            if (r.empty()) continue;
            result = min(result, offer.back() + shoppingOffers(price, special, r));
        }
        return result;
    }
    vector<int> can(const vector<int>& needs, const vector<int>& offer) {
        vector<int> r(needs.size(), 0);
        for (int i = 0, n = needs.size(); i < n; ++i) {
            if (offer[i] > needs[i]) return vector<int>();
            r[i] = needs[i] - offer[i];
        }
        return r;
    }
};
```