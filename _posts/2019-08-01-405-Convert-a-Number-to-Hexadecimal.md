---
layout: post
title:  "405. Convert a Number to Hexadecimal"
date: 2019-08-01 20:19:00 -0400
categories: articles
---
Given an integer, write an algorithm to convert it to hexadecimal. For negative integer, two’s complement method is used.

Note:

All letters in hexadecimal (a-f) must be in lowercase.
The hexadecimal string must not contain extra leading 0s. If the number is zero, it is represented by a single zero character '0'; otherwise, the first character in the hexadecimal string will not be the zero character.
The given number is guaranteed to fit within the range of a 32-bit signed integer.
You must not use any method provided by the library which converts/formats the number to hex directly.
Example 1:
```
Input:
26

Output:
"1a"
```
Example 2:
```
Input:
-1

Output:
"ffffffff"
```
```c++
class Solution {
public:
    string toHex(int n) {
        long num = n;
        if( num < 0 )
             num = INT_MAX + num + INT_MAX + 2;
        string res;
        if(!n)
            return "0";
        string dic = "0123456789abcdef";
        while(num){
            res = dic[num%16] + res;
            num = num/16;
        }
        return res;
    }
};
```