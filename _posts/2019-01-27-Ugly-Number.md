---
layout: post
title:  "263. Ugly Number"
date: 2019-01-27 14:48:23 -0400
categories: articles
---
Write a program to check whether a given number is an ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5.

Example 1:
```
Input: 6
Output: true
Explanation: 6 = 2 × 3
```
Example 2:
```
Input: 8
Output: true
Explanation: 8 = 2 × 2 × 2
```
Example 3:
```
Input: 14
Output: false 
Explanation: 14 is not ugly since it includes another prime factor 7.
```
Note:
```
1. is typically treated as an ugly number.
2. Input is within the 32-bit signed integer range: [−231,  231 − 1].
```
# Funciton signature
```c++
class Solution {
public:
    bool isUgly(int num) {
        
    }
};
```
# 题意
判断是不是一个丑数，如果它的质因数只包含2，3，5的话就是，否则就不是。
# 想法
那就去除2，3，5看看最后能不能出尽。

__记住，有些事后不一定要在一个循环里做事情。
如果要用不同的处理，可以在不同的循环中进行。__
# 尝试解解
```c++
// Time out
class Solution {
public:
    bool isUgly(int num) {
    	while ( num%2 == 0 || num%3 == 0 || num%5 == 0) {
    		if ( num%2 == 0)
    			num = num/2;
    		else if ( num % 3 == 0)
    			num = num/3;
    		else if ( num % 5 == 0)
    			num = num/5;
    	}
    	return num > 5;
    }
};
```
```c++
// Accepted !
class Solution {
public:
    bool isUgly(int num) {
        if (num == 0) return false;
    	while( num%2 == 0 ) num = num/2;
        while( num%3 == 0 )  num = num/3;            
        while( num%5 == 0 ) num = num/5;
    	return num == 1;
    }
};
```
