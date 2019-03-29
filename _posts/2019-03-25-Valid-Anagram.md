---
layout: post
title:  "242. Valid Anagram"
date: 2019-03-25 21:41:23 -0400
categories: articles
---
Given two strings s and t , write a function to determine if t is an anagram of s.

Example 1:
```
Input: s = "anagram", t = "nagaram"
Output: true
```
Example 2:
```
Input: s = "rat", t = "car"
Output: false
```
Note:
You may assume the string contains only lowercase alphabets.

Follow up:
What if the inputs contain unicode characters? How would you adapt your solution to such case?

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int A[26] = {0};
        int B[26] = {0};
        for ( auto i : s ) A[i-'a']++;
        for ( auto i : t ) B[i-'a']++;
        for ( int i = 0; i < 26; i++ ) {
            if (A[i] != B[i]) return false;
        }
        return true;
    }
};
```