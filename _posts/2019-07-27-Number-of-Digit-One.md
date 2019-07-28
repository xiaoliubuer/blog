---
layout: post
title:  "233. Number of Digit One"
date: 2019-07-27 19:48:00 -0400
categories: articles
---

Given an integer n, count the total number of digit 1 appearing in all non-negative integers less than or equal to n.

Example:
```
Input: 13
Output: 6 
```
Explanation: Digit 1 occurred in the following numbers: 1, 10, 11, 12, 13.


```c++
class Solution {
public:
    int countDigitOne(int n) {
        int res = 0;
        long a = 1; 
        long b = 1;
        while (n > 0) {
            res += (n + 8) / 10 * a + (n % 10 == 1) * b;
            b += n % 10 * a;
            a *= 10;
            n /= 10;
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int countDigitOne(int n) {
        if (n <= 0)
            return 0;
        
        long long int total = 0;
        int high = n, low = 0, cur = 0;
        int number = 1, k = 1;
        
        while (high > 0)
        {
            cur = high % 10;
            high /= 10;
            
            if (cur == 0)
                total += high * number;
            else if (cur == 1)
                total += ( high * number + (low + 1) );
            else
                total += (high + 1) * number;
                
            number *= 10;
            low += cur * k;
            k *= 10;
        }
        
        return total;
    }
};
```