---
layout: post
title:  "459. Repeated Substring Pattern"
date: 2019-08-05 19:25:00 -0400
categories: articles
---
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

Example 1:
```
Input: "abab"
Output: True
Explanation: It's the substring "ab" twice.
```
Example 2:
```
Input: "aba"
Output: False
```
Example 3:
```
Input: "abcabcabcabc"
Output: True
```
Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)



```c++
class Solution {
public:
    bool repeatedSubstringPattern(string& s) {
        for (size_t length = s.size() / 2; length >= 1; --length) {
            if (s.size() % length != 0) { continue; }
            if (isSubstringPattern(s, length)) {
                return true;
            }
        }
        return false;
    }
    
    bool isSubstringPattern(const string& s, const size_t length) {
        size_t begin = length;
        while (begin < s.size()) {
            if (memcmp(&s[0], &s[begin], length) != 0) {
                return false;
            }
            begin += length;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        int n = 2;
        int len = s.size();
        if (len < 2) return false;
        while (n < len)
        {
            if(len % n == 0)
            {
                int k = len / n;
                bool is_repeated = true;
                for (int i = 1; i < k && is_repeated; ++i)
                {
                    for (int j = 0; j < n; ++j)
                    {
                        if (s[j] != s[i*n+j])
                        {
                            is_repeated = false;
                            break;
                        }
                    }
                }
                if (is_repeated) return true;
            }
            ++n;
        }
        for (int i = 1; i < len; ++i)
        {
            if (s[0] != s[i]) return false;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).find(s, 1) < s.size();
    }
};
```
```c++
class Solution {
public:
    bool repeatedSubstringPattern(string s) {
        return (s + s).substr(1, s.size()*2-2).find(s) != -1;
    }
};
```