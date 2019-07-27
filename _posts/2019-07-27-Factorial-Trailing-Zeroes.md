---
layout: post
title:  "172. Factorial Trailing Zeroes"
date: 2019-07-27 18:19:00 -0400
categories: articles
---
Given an integer n, return the number of trailing zeroes in n!.
Example 1:
```
Input: 3
Output: 0
Explanation: 3! = 6, no trailing zero.
```
Example 2:
```
Input: 5
Output: 1
Explanation: 5! = 120, one trailing zero.
```
```c++
class Solution {
public:
int trailingZeroes(int n) {
    int result = 0;
    for(long long i=5; n/i>0; i*=5){
        result += (n/i);
    }
    return result;
}
};
```