---
layout: post
title:  "* 43. Multiply Strings"
date: 2019-01-05 15:12:23 -0400
categories: articles
---
Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Example 1:
```
Input: num1 = "2", num2 = "3"
Output: "6"
```
Example 2:
```
Input: num1 = "123", num2 = "456"
Output: "56088"
```
Note:

The length of both num1 and num2 is < 110.
Both num1 and num2 contain only digits 0-9.
Both num1 and num2 do not contain any leading zero, except the number 0 itself.
You must not use any built-in BigInteger library or convert the inputs to integer directly.
# Function signature
```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        
    }
};
```
# 题意
两个非负整数num1和num2用string表示，返回num1和num2的product，同样也用stirng来表示。
# 想法
不可以转化成整数做乘法，应为太大了。那能怎么做呢？？
卧槽，真的没思路啦～不能转化成int，单纯用字符怎么做啊？？
# Shame answer
```c++
class Solution {
public:
string multiply(string num1, string num2) {
    string sum(num1.size() + num2.size(), '0');
    
    for (int i = num1.size() - 1; 0 <= i; --i) {
        int carry = 0;
        for (int j = num2.size() - 1; 0 <= j; --j) {
            int tmp = (sum[i + j + 1] - '0') + (num1[i] - '0') * (num2[j] - '0') + carry;
            sum[i + j + 1] = tmp % 10 + '0';
            carry = tmp / 10;
        }
        sum[i] += carry;
    }
    
    size_t startpos = sum.find_first_not_of("0");
    if (string::npos != startpos) {
        return sum.substr(startpos);
    }
    return "0";
}
};
```