---
layout: post
title:  "634. Find the Derangement of An Array"
date: 2019-08-18 14:56:00 -0400
categories: articles
---	
In combinatorial mathematics, a derangement is a permutation of the elements of a set, such that no element appears in its original position.

There's originally an array consisting of n integers from 1 to n in ascending order, you need to find the number of derangement it can generate.

Also, since the answer may be very large, you should return the output mod 109 + 7.

Example 1:
```
Input: 3
Output: 2
Explanation: The original array is [1,2,3]. The two derangements are [2,3,1] and [3,1,2].
```
Note:
```
n is in the range of [1, 106].
```
```c++
class Solution {
    typedef long long ll;
    const ll MOD = 1e9+7;
public:
    int findDerangement(int N) {
        if (N < 4) return N - 1;
        ll Fn_2 = 0, Fn_1 = 1, Fn = 2;
        for (int n=4; n<=N; n++) {
            Fn_2 = Fn_1;
            Fn_1 = Fn;
            Fn = (n-1) * (Fn_1 + Fn_2) % MOD;
        }
        return Fn;
    }
};
```
```c++
class Solution {
public:
    int findDerangement(int n) {
        if(n<=1) return 0;
        const int di=pow(10, 9)+7;
        long dp0=0, dp1=1, dp=1;
        for(int i=3;i<=n;i++) {
            dp=((i-1)*dp1+(i-1)*dp0)%di;
            dp0=dp1, dp1=dp;
        }
        return dp;
    }
};
```