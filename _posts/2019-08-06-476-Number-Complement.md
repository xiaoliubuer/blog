---
layout: post
title:  "476. Number Complement"
date: 2019-08-06 19:42:00 -0400
categories: articles
---

Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation.

Note:
The given integer is guaranteed to fit within the range of a 32-bit signed integer.
You could assume no leading zero bit in the integer’s binary representation.
Example 1:
```
Input: 5
Output: 2
Explanation: The binary representation of 5 is 101 (no leading zero bits), and its complement is 010. So you need to output 2.
```
Example 2:
```
Input: 1
Output: 0
Explanation: The binary representation of 1 is 1 (no leading zero bits), and its complement is 0. So you need to output 0.
```
```c++
class Solution {
public:
    int findComplement(int num) {
        long n=1;
        for(int i=0;n<=num;i++){
            n *=2;
        }
        return int(n-1-num);
    }
};
```
```c++
class Solution {
public:
    int findComplement(const int& num) {
        return num ^ INT_MAX >> shift(num);
    }
    
    //returns how many should be shifted before xor
    int shift(int num) {
        int count = 0;
        while (num != 0) {
            count++;
            num >>= 1;
        }
        return 31 - count;
    }
};
```