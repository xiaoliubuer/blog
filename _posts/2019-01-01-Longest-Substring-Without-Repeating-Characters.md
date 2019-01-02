---
layout: post
title:  "03. Longest Substring Without Repeating Characters"
date: 2019-01-01 07:45:23 -0400
categories: articles
---
Given a string, find the length of the longest substring without repeating characters.

Example 1:
```
Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
```
Example 2:
```
Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
```
Example 3:
```
Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
```
# 题意
给了一个string，找到最长的substring没有重复字符的。__注意：substring 和 subsequence 的区别。__

# Function signature
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        
    }
};
```
# 想法
slidling window
# Key code
```c++
int mark[256] = {0};
```
# 参考答案
```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        if (s.length() == 0) return 0;
        
        int slow = 0;
        int i = 0;
        int mark[256] = {0};
        int res = 0;
        while (i < s.length()) {
            // fast move condition
            if ( mark[s[i]] == 0 ){
                res = max( res, i - slow + 1);
                mark[s[i]]++;
                i++;
                continue;
            }
            else {
                mark[s[slow]]--;
                slow++;
            }
        }
        return res;
    }
};
```