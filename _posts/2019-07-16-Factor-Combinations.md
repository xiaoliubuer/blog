---
layout: post
title:  "254. Factor Combinations"
date: 2019-07-16 20:22:00 -0400
categories: articles
---

Numbers can be regarded as product of its factors. For example,

8 = 2 x 2 x 2;
  = 2 x 4.
Write a function that takes an integer n and return all possible combinations of its factors.

Note:

You may assume that n is always positive.
Factors should be greater than 1 and less than n.
Example 1:
```
Input: 1
Output: []
Example 2:

Input: 37
Output:[]
```
Example 3:
```
Input: 12
Output:
[
  [2, 6],
  [2, 2, 3],
  [3, 4]
]
```
Example 4:
```
Input: 32
Output:
[
  [2, 16],
  [2, 2, 8],
  [2, 2, 2, 4],
  [2, 2, 2, 2, 2],
  [2, 4, 4],
  [4, 8]
]
```
```c++
class Solution {
public:
    vector<vector<int>> getFactors(int n) {
        vector<vector<int>> res;
        vector<int> temp;
        if ( n == 1 ) return res;
        helper(2, n, temp, res);
        return res;
    }
    void helper(int s, int n, vector<int>& temp, vector<vector<int>>& res){
        if ( n <= s ) return;
        
        for ( int i = s; i <= sqrt(n); i++ ){
            if ( n % i == 0 ) {
                temp.push_back(i);
                temp.push_back( n/i );
                res.push_back(temp);
                temp.pop_back(); // Interesting this line, must be before helper
                helper(i, n/i, temp, res);
                temp.pop_back();
            }
        }
    }
};
```