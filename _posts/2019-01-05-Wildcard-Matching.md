---
layout: post
title:  "* 44. Wildcard Matching"
date: 2019-01-05 15:28:23 -0400
categories: articles
---
Given an input string (s) and a pattern (p), implement wildcard pattern matching with support for '?' and '*'.
```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).
```
The matching should cover the entire input string (not partial).

Note:
```
s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like ? or *.
```
Example 1:
```
Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
```
Example 2:
```
Input:
s = "aa"
p = "*"
Output: true
Explanation: '*' matches any sequence.
```
Example 3:
```
Input:
s = "cb"
p = "?a"
Output: false
Explanation: '?' matches 'c', but the second letter is 'a', which does not match 'b'.
```
Example 4:
```
Input:
s = "adceb"
p = "*a*b"
Output: true
Explanation: The first '*' matches the empty sequence, while the second '*' matches the substring "dce".
```
Example 5:
```
Input:
s = "acdcb"
p = "a*c?b"
Output: false
```
# Function signature
```
class Solution {
public:
    bool isMatch(string s, string p) {
        
    }
};
```
# 题意
给一个string s和pattern

# 想法
真的不是很会。

# Shame answer
```c++
 bool isMatch(const char *s, const char *p) {
        const char* star=NULL;
        const char* ss=s;
        while (*s){
            //advancing both pointers when (both characters match) or ('?' found in pattern)
            //note that *p will not advance beyond its length 
            if ((*p=='?')||(*p==*s)){s++;p++;continue;} 

            // * found in pattern, track index of *, only advancing pattern pointer 
            if (*p=='*'){star=p++; ss=s;continue;} 

            //current characters didn't match, last pattern pointer was *, current pattern pointer is not *
            //only advancing pattern pointer
            if (star){ p = star+1; s=++ss;continue;} 

           //current pattern pointer is not star, last patter pointer was not *
           //characters do not match
            return false;
        }

       //check for remaining characters in pattern
        while (*p=='*'){p++;}

        return !*p;  
    }
```
```c++
// 答案翻版
class Solution {
public:
    bool isMatch(string s, string p) {
        int sc = 0, pc = 0, match = 0, startIdx = -1;
        while (pc < p.size()){
            if (pc < p.size() && (p[pc] == '?' || s[sc] == p[pc])){
                sc++;
                pc++;
            }
            else if (pc < p.size() && p[pc] == '*'){
                startIdx = pc;
                match = sc;
                pc++;
            }
            else if (startIdx != -1){
                p = startIdx + 1;
                match++;
                sc = match;
            }
            else
                return false;
        }
        while (pc < p.size() && p[pc] == '*')
            pc++;
        return pc == p.size();
    }
};
```
