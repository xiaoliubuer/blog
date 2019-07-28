---
layout: post
title:  "247. Strobogrammatic Number II"
date: 2019-07-27 20:10:00 -0400
categories: articles
---

A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Find all strobogrammatic numbers that are of length = n.

Example:
```
Input:  n = 2
Output: ["11","69","88","96"]
```
```c++

class Solution {
public:
    // 直接从两边往中间搜索
    vector<string> findStrobogrammatic(int n) {
        return helper(n , n);
    }
    vector<string> helper(int m, int n){
        // m 指定了返回串的长度
        if(m == 0) {
            return vector<string>({""});
        }
        if(m == 1) {
            return vector<string>({"0", "1", "8"});
        }
        vector<string> tmp = helper(m - 2, n), res;
        for(auto item : tmp){
            if(m != n) { // "00" 不是合法的数字
                res.push_back("0" + item + "0");
            }
            res.push_back("1" + item + "1");
            res.push_back("6" + item + "9");
            res.push_back("9" + item + "6");
            res.push_back("8" + item + "8");
        }
        return res;
    }
};
```