---
layout: post
title:  "214. Shortest Palindrome"
date: 2019-07-27 18:49:00 -0400
categories: articles
---

Given a string s, you are allowed to convert it to a palindrome by adding characters in front of it. Find and return the shortest palindrome you can find by performing this transformation.

Example 1:
```
Input: "aacecaaa"
Output: "aaacecaaa"
```
Example 2:
```
Input: "abcd"
Output: "dcbabcd"
```
```c++
class Solution {
public:
string shortestPalindrome(string s)
{
    int n = s.size();
    string rev(s);
    reverse(rev.begin(), rev.end());
    int j = 0;
    for (int i = 0; i < n; i++) {
        if (s.substr(0, n - i) == rev.substr(i))
            return rev.substr(0, i) + s;
    }
    return "";
}
}
// n^2
```
```c++
class Solution {
public:
string shortestPalindrome(string s)
{
    int n = s.size();
    int i = 0;
    for (int j = n - 1; j >= 0; j--) {
        if (s[i] == s[j])
            i++;
    }
    if (i == n)
        return s;
    string remain_rev = s.substr(i, n);
    reverse(remain_rev.begin(), remain_rev.end());
    return remain_rev + shortestPalindrome(s.substr(0, i)) + s.substr(i);
}
}
// Recursion
```
```c++
class Solution {
public:
    string shortestPalindrome(string s) {
        string rev_s = s;
        reverse(rev_s.begin(), rev_s.end());
        string l = s + "#" + rev_s;
        
        vector<int> p(l.size(), 0);
        for (int i = 1; i < l.size(); i++) {
            int j = p[i - 1];
            while (j > 0 && l[i] != l[j])
                j = p[j - 1];
            p[i] = (j += l[i] == l[j]);
        }
        
        return rev_s.substr(0, s.size() - p[l.size() - 1]) + s;
    }
};
```