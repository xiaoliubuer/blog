---
layout: post
title:  "266. Palindrome Permutation"
date: 2019-03-26 18:40:23 -0400
categories: articles
---
Given a string, determine if a permutation of the string could form a palindrome.

Example 1:
```
Input: "code"
Output: false
```
Example 2:
```
Input: "aab"
Output: true
```
Example 3:
```
Input: "carerac"
Output: true
```

```c++
class Solution {
public:
    bool canPermutePalindrome(string s) {
        bitset<26> myset;
        for ( auto i : s ) myset[ i - 'a'].flip();
        return myset.count() <= 1;
    }
};
```