---
layout: post
title:  "290. Word Pattern"
date: 2019-07-16 20:22:00 -0400
categories: articles
---
Given a pattern and a string str, find if str follows the same pattern.
Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty word in str.
Example 1:
```
Input: pattern = "abba", str = "dog cat cat dog"
Output: true
```
Example 2:
```
Input:pattern = "abba", str = "dog cat cat fish"
Output: false
```
Example 3:
```
Input: pattern = "aaaa", str = "dog cat cat dog"
Output: false
```
Example 4:
```
Input: pattern = "abba", str = "dog dog dog dog"
Output: false
```
Notes:
```
You may assume pattern contains only lowercase letters, and str contains lowercase letters that may be separated by a single space.
```



```c++
class Solution {
public:
    bool wordPattern(string pattern, string str) {
        map<char, int> sc;
        map<string, int> wc;
        istringstream in(str);
        int i = 0, n = pattern.size();
        for ( string word; in >> word; i++ ) {
            if ( i == n || sc[pattern[i]] != wc[word]) return false;
            sc[pattern[i]] = wc[word] = i + 1;
        }
        return i == n;
    }
};
```