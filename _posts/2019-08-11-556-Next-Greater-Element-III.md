---
layout: post
title:  "556. Next Greater Element III"
date: 2019-08-11 16:22:00 -0400
categories: articles
---
Given a positive 32-bit integer n, you need to find the smallest 32-bit integer which has exactly the same digits existing in the integer n and is greater in value than n. If no such positive 32-bit integer exists, you need to return -1.

Example 1:
```
Input: 12
Output: 21
```
Example 2:
```
Input: 21
Output: -1
```
```c++
class Solution {
public:
    int nextGreaterElement(int n) {
        string t = to_string(n);
        if(!next_permutation(t.begin(), t.end())) return -1;
        return (stoll(t) >= INT_MAX) ? -1 : stoi(t);
    }
    
};
```
```c++
class Solution {
public:
    int nextGreaterElement(int n) {
        string s = to_string(n);
        int size = s.size(), i = size-1;
        for(; i > 0; --i) 
            if(s[i] > s[i-1]) break;
        if(i == 0) return -1;
		reverse(s.begin() + i, s.end());
        for(int j = i; j < size; ++j) 
            if(s[i-1] < s[j]) {
                swap(s[i-1], s[j]);
                break;
            }
        int64_t t = stol(s);
        return t > INT32_MAX ? -1 : t;
    }
};
```