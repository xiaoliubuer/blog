---
layout: post
title:  "357. Count Numbers with Unique Digits"
date: 2019-07-31 19:59:00 -0400
categories: articles
---

Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

Example:
```
Input: 2
Output: 91 
```
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding 11,22,33,44,55,66,77,88,99


```c++
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        static const int optCount[] = {9, 9, 8, 7, 6, 5, 4, 3, 2, 1};
        int ans = 10;
        int product = 9;
        
        if (n == 0) {
            return 1;
        }
        
        for (int i = 1; i < n; i++) {
            product = product * optCount[i];
            ans += product;
        }
        
        return ans;
    }
};
```
```c++
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        int a[11];
        a[0]=1;
        a[1]=10;
        for(int i=2;i<=10;i++)
        {
            a[i]=a[i-1];
            int m=9;
            int x=9;
            for(int j=1;j<i;j++)
            {
                m=m*x;
                x--;
            }
            a[i]+=m;
        }
        if(n<=10)
        {
            return a[n];
        }
        else
        {
            return a[10];
        }
        
    }
};
```