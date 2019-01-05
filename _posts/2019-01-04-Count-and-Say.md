---
layout: post
title:  "? 38. Count and Say"
date: 2019-01-04 21:21:23 -0400
categories: articles
---
The count-and-say sequence is the sequence of integers with the first five terms as following:
```
1.     1
2.     11
3.     21
4.     1211
5.     111221
```
1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
# Function signature
class Solution {
public:
    string countAndSay(int n) {
        
    }
};

# 题意
就是看着给的顺序来数，然后输出几个几，几个几的东西。

# 参考答案
```c++
class Solution {
public:
string countAndSay(int n) {
    if (n == 0) return "";
    string res = "1";
    while (--n) {
        string cur = "";
        for (int i = 0; i < res.size(); i++) {
            int count = 1;
             while ((i + 1 < res.size()) && (res[i] == res[i + 1])){
                count++;    
                i++;
            }
            cur += to_string(count) + res[i];
        }
        res = cur;
    }
    return res;
}
};```