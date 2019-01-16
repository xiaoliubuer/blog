---
layout: post
title:  "@ 58. Length of Last Word"
date: 2019-01-07 21:38:23 -0400
categories: articles
---
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.

If the last word does not exist, return 0.

Note: A word is defined as a character sequence consists of non-space characters only.

Example:
```
Input: "Hello World"
Output: 5
```
# Function signature
```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        
    }
};
```
# 题意

# Answer
```c++
class Solution {
public:
    int lengthOfLastWord(string s) {
        int len = 0, tail = s.length() - 1;
        while (tail >= 0 && s[tail] == ' ') tail--;
        while (tail >= 0 && s[tail] != ' ') {
            len++;
            tail--;
        }
        return len;
    }
};
```