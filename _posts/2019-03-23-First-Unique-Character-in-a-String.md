---
layout: post
title:  "387. First Unique Character in a String"
date: 2019-03-23 08:48:23 -0400
categories: articles
---
Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.

Examples:
```
s = "leetcode"
return 0.

s = "loveleetcode",
return 2.
```
Note: You may assume the string contain only lowercase letters.

```c++
class Solution {
public:
    int firstUniqChar(string s) {
        if (s.size() == 0) return -1;
        int buff[26] = {0};
        
        for (auto i : s) {
            buff[ i - 'a']++;
        }
        for (int i = 0; i < s.size(); i++) {
            if (buff[s[i] - 'a'] == 1) return i;
        }
        return -1;
    }
};
```