---
layout: post
title:  "709. To Lower Case"
date: 2019-01-21 17:41:23 -0400
categories: articles
---
Implement function ToLowerCase() that has a string parameter str, and returns the same string in lowercase.

Example 1:
```
Input: "Hello"
Output: "hello"
```
Example 2:
```
Input: "here"
Output: "here"
```
Example 3:
```
Input: "LOVELY"
Output: "lovely"
```
# Function signature
```c++
class Solution {
public:
    string toLowerCase(string str) {
        
    }
};
```
# 题意
实现一个函数，使输入的stirng变成小写字母。
# 想法
遍历string。每个char + 32
# 尝试解解
```c++
// Accepted !!
class Solution {
public:
    string toLowerCase(string str) {
    int gap = 'a' - 'A';
    for (int i = 0; i < str.size(); i++ ){
        if (str[i] >= 'A' && str[i] <= 'Z')
            str[i] += gap;
    }
    return str;
    }
};
```