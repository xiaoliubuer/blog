---
layout: post
title:  "678. Valid Parenthesis String"
date: 2019-08-18 17:05:00 -0400
categories: articles
---

Given a string containing only three types of characters: '(', ')' and ' * ', write a function to check whether this string is valid. We define the validity of a string by these rules:
```
Any left parenthesis '(' must have a corresponding right parenthesis ')'.
Any right parenthesis ')' must have a corresponding left parenthesis '('.
Left parenthesis '(' must go before the corresponding right parenthesis ')'.
'*' could be treated as a single right parenthesis ')' or a single left parenthesis '(' or an empty string.
An empty string is also valid.
```
Example 1:
```
Input: "()"
Output: True
```
Example 2:
```
Input: "(*)"
Output: True
```
Example 3:
```
Input: "(*))"
Output: True
```
Note:
```
The string size will be in the range [1, 100].
```
```c++
class Solution {
public:
    bool checkValidString(string s) {
        int cntstarl = 0;
        int cntstarr = 0;
        int cntleft = 0;
        for(char c:s)
        {
            if(c=='(')    cntleft++;
            else if(c==')')
            {
                if(cntleft>0) {cntleft--;cntstarr = min(cntstarr,cntleft);}
                else if(cntstarl>0) {cntstarl--;}
                else return false;
            }
            else
            {
                cntstarl++;
                if(cntstarr<cntleft)    cntstarr++;
            }
        }
        return cntleft<=cntstarr;
    }
};
```