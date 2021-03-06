---
layout: post
title:  "693. Binary Number with Alternating Bits"
date: 2019-08-18 18:11:00 -0400
categories: articles
---

Given a positive integer, check whether it has alternating bits: namely, if two adjacent bits will always have different values.

Example 1:
```
Input: 5
Output: True
Explanation:
The binary representation of 5 is: 101
```
Example 2:
```
Input: 7
Output: False
Explanation:
The binary representation of 7 is: 111.
```
Example 3:
```
Input: 11
Output: False
Explanation:
The binary representation of 11 is: 1011.
```
Example 4:
```
Input: 10
Output: True
Explanation:
The binary representation of 10 is: 1010.
```
```c++
class Solution {
public:
    bool hasAlternatingBits(int n) {
        n ^= (n >> 1);
        return n == INT_MAX || (n & (n + 1)) == 0;
    }
};
```
```c++
class Solution {
public:
    bool hasAlternatingBits(int n) {
        int temp = n;
        temp = temp >> 1;
        
        int result = temp ^ n;
        
        long int bit = 1;
        while ( bit <=  n)
            bit = bit << 1;
        
        bit = bit - 1;
        
        
        return (bit == result);
    }
};
```