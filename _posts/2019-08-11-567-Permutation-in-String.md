---
layout: post
title:  "567. Permutation in String"
date: 2019-08-11 17:13:00 -0400
categories: articles
---

Given two strings s1 and s2, write a function to return true if s2 contains the permutation of s1. In other words, one of the first string's permutations is the substring of the second string.

Example 1:
```
Input: s1 = "ab" s2 = "eidbaooo"
Output: True
Explanation: s2 contains one permutation of s1 ("ba").
```
Example 2:
```
Input:s1= "ab" s2 = "eidboaoo"
Output: False
```
Note:
```
The input strings only contain lower case letters.
The length of both given strings is in range [1, 10,000].
```
```c++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        const int size1 = s1.size(), size2 = s2.size();
        if(size1 > size2) return false;
        vector<int> m(27, 0);
        for(const auto &c: s1) --m[c - 'a'];
        int i = 0, j = size1-1; 
        for(int i = 0; i <= j; ++i) ++m[s2[i] - 'a'];
        while(j < size2 - 1) {
            if(equal(m.begin(), m.end(), m.rbegin())) return true;
            ++m[s2[++j] - 'a'];
            --m[s2[i++] - 'a'];
        }
        return equal(m.begin(), m.end(), m.rbegin());
    }
};
```