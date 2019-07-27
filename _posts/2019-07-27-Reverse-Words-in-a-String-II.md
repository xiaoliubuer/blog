---
layout: post
title:  "186. Reverse Words in a String II"
date: 2019-07-27 18:13:00 -0400
categories: articles
---

Given an input string , reverse the string word by word. 

Example:
```
Input:  ["t","h","e"," ","s","k","y"," ","i","s"," ","b","l","u","e"]
Output: ["b","l","u","e"," ","i","s"," ","s","k","y"," ","t","h","e"]
```
Note: 
```
A word is defined as a sequence of non-space characters.
The input string does not contain leading or trailing spaces.
The words are always separated by a single space.
Follow up: Could you do it in-place without allocating extra space?
```
```c++
class Solution {
public:
    void reverseWords(vector<char>& s) {
        reverse(s.begin(), s.end());
        int n = s.size();
        for (int i = 0, st = 0, en = 0; i < n; i++) {
            st = i;
            while (i < n && s[i] != ' ') {
                i++;
            }
            en = i;
            reverse(s.begin() + st, s.begin() + en);
        }
        
    }
};
```
```c++
class Solution {
public:
    void reverseWords(vector<char>& s) {
        reverse(s.begin(), s.end());
        int n = s.size(), l = 0, r = 0;
        while (r < n) {
            while (r < n && !isspace(s[r])) r++;
            reverse(s.begin() + l, s.begin() + r); 
            l = ++r;
        }  
    }
};
```