---
layout: post
title:  "171. Excel Sheet Column Number"
date: 2019-03-28 19:54:23 -0400
categories: articles
---
Given a column title as appear in an Excel sheet, return its corresponding column number.

For example:
```
    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
    ...
```
Example 1:
```
Input: "A"
Output: 1
```
Example 2:
```
Input: "AB"
Output: 28
```
Example 3:
```
Input: "ZY"
Output: 701
```

```c++
class Solution {
public:
    int titleToNumber(string s) {
        int res = 0;
        for (auto i : s){
            res = res * 26 + (i - 'A') + 1;
        }
        return res;
    }
};
```