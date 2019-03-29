---
layout: post
title:  "67. Add Binary"
date: 2019-03-24 20:25:23 -0400
categories: articles
---
Given two binary strings, return their sum (also a binary string).

The input strings are both non-empty and contains only characters 1 or 0.

Example 1:
```
Input: a = "11", b = "1"
Output: "100"
```
Example 2:
```
Input: a = "1010", b = "1011"
Output: "10101"
```

```c++
class Solution {
public:
    string addBinary(string a, string b) {
        char carry = 0;
        
        string res;
        int l = a.size() - 1;
        int r = b.size() - 1;
        while ( l >= 0 || r >= 0 ) {
            int aVal, bVal;
            aVal = l >= 0 ? a[l] - '0' : 0;
            bVal = r >= 0 ? b[r] - '0' : 0;
            
            int sum = aVal + bVal + carry;
            int val = sum % 2;
            carry = sum / 2;
            res.push_back( val + '0');
            if ( l >= 0 ) l--;
            if ( r >= 0 ) r--;
        }
        if ( carry == 1 ) res.push_back('1');
        reverse(res.begin(), res.end());
        return res;
    }
};
```