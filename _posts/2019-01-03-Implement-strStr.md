---
layout: post
title:  "28. Implement strStr()"
date: 2019-01-03 07:39:23 -0400
categories: articles
---
Implement strStr().

Return the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.

Example 1:
```
Input: haystack = "hello", needle = "ll"
Output: 2
```
Example 2:
```
Input: haystack = "aaaaa", needle = "bba"
Output: -1
```
Clarification:

What should we return when needle is an empty string? This is a great question to ask during an interview.

For the purpose of this problem, we will return 0 when needle is an empty string. This is consistent to C's strstr() and Java's indexOf().
# Function signature
```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
        
    }
};
```
# 题意
返回草堆中的第一根针。如果没有就返回-1.
# Thinking
Search by first letter of the needle with, if match, get the substring of the haystack. Then compare the the substring with needle.

```c++
class Solution {
public:
    int strStr(string haystack, string needle) {
    	int m = haystack.size(), n = needle.size();
    	if ( n == 0) return 0;
    	for ( int i = 0; i <= m - n; i++ ) { // must be <=
    		if (haystack[i] == needle[0]) {
    			string sub = haystack.substr(i, n);
    			if (sub == needle) return i;
    		}
    	}
        return -1;
    }
};
```
```c++
//Trick soluiton
class Solution {
public:
    int strStr(string haystack, string needle) {
        if ( 0 == needle.size()) return 0;
        int idx = -1;
        idx = haystack.find(needle);
        return idx;   
    }
};
```
