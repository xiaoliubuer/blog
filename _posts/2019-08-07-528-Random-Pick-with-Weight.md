---
layout: post
title:  "528. Random Pick with Weight"
date: 2019-08-07 20:56:00 -0400
categories: articles
---
Given an array w of positive integers, where w[i] describes the weight of index i, write a function pickIndex which randomly picks an index in proportion to its weight.

Note:
```
1 <= w.length <= 10000
1 <= w[i] <= 10^5
pickIndex will be called at most 10000 times.
```
Example 1:
```
Input: 
["Solution","pickIndex"]
[[[1]],[]]
Output: [null,0]
```
Example 2:
```
Input: 
["Solution","pickIndex","pickIndex","pickIndex","pickIndex","pickIndex"]
[[[1,3]],[],[],[],[],[]]
Output: [null,0,1,1,1,0]
```
Explanation of Input Syntax:
```
The input is two lists: the subroutines called and their arguments. Solution's constructor has one argument, the array w. pickIndex has no arguments. Arguments are always wrapped with a list, even if there aren't any.
```
```c++
class Solution {
    vector<int> preSum;
    int sum;
public:
    Solution(vector<int>& w) {
        sum = 0;
        for(int c : w) {
            sum += c;
            preSum.push_back(sum);
        }
    }
    
    int pickIndex() {
        double rnd = (0.0+rand())/INT_MAX*sum; // random double in [0, sum]
        int l = 0, u = preSum.size();
        while(l < u) {
            int m = l + (u-l)/2;
            if (preSum[m] <= rnd) l = m+1;
            else u = m;
        }
        return l;
    }
};
```
```c++
class Solution 
{
private:
    vector<int> sum;
public:
    Solution(vector<int>& w) 
    {
        sum.resize(w.size());
        sum[0] = w[0];
        for(int i=1; i < w.size(); ++i)
            sum[i] = sum[i-1] + w[i];
        
    }
    
    int pickIndex() 
    {
        int val = rand() % sum.back();
        auto idx = upper_bound(sum.begin(), sum.end(), val);
        
        return idx - sum.begin();
    }
};
```