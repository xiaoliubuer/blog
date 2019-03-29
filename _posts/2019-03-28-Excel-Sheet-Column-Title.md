---
layout: post
title:  "168. Excel Sheet Column Title"
date: 2019-03-28 19:45:23 -0400
categories: articles
---
Given a positive integer, return its corresponding column title as appear in an Excel sheet.

For example:
```
    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
```
Example 1:
```
Input: 1
Output: "A"
```
Example 2:
```
Input: 28
Output: "AB"
```
Example 3:
```
Input: 701
Output: "ZY"
```

```c++
class Solution {
public:
    string convertToTitle(int n) {
        string res;
        if ( n == 1 ) return "A";
        while( n > 0 ) {
            res.push_back((n - 1) % 26 + 'A') ;
            n = (n - 1) / 26;
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```