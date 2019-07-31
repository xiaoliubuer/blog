---
layout: post
title:  "326. Power of Three"
date: 2019-07-30 19:40:00 -0400
categories: articles
---
Given an integer, write a function to determine if it is a power of three.

Example 1:
```
Input: 27
Output: true
```
Example 2:
```
Input: 0
Output: false
```
Example 3:
```
Input: 9
Output: true
```
Example 4:
```
Input: 45
Output: false
```
```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        while(n>1){
            if(n%3!=0){
                return false;
            }
            n/=3;
        }
        return n==1;
    }
};
```