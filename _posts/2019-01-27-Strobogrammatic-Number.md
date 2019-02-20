---
layout: post
title:  "246. Strobogrammatic Number"
date: 2019-01-27 16:47:23 -0400
categories: articles
---
A strobogrammatic number is a number that looks the same when rotated 180 degrees (looked at upside down).

Write a function to determine if a number is strobogrammatic. The number is represented as a string.

Example 1:
```
Input:  "69"
Output: true
```
Example 2:
```
Input:  "88"
Output: true
```
Example 3:
```
Input:  "962"
Output: false
```
# Function signature
```c++
class Solution {
public:
    bool isStrobogrammatic(string num) {
        
    }
};
```
# 题意
strobogrammatic就是个颠倒180度依然一样的数。比如 69，88
# 想法
卧槽，这是考脑经急转弯儿呢吧？？
[1, 1] [6, 9], [8, 8], [9, 6], [0, 0]
拿map存了，然后比就行了
# 尝试解解
```c++
// Accepted!!
class Solution {
public:
    bool isStrobogrammatic(string num) {
    	if ( num.size() == 0 ) return false;
    	unordered_map<char, char> dir({ {'1', '1'}, {'6','9'}, {'8', '8'}, {'9', '6'}, {'0', '0'} });
    	// unordered_map<char, char> dir = { {'1', '1'}, {'6','9'}, {'8', '8'}, {'9', '6'}, {'0', '0'} }; 初始化map，两种方法都可以。
    	int n = num.size();
    	int l = 0, r = n - 1;
    	if (n > 1 && num[r] == '0') return false;
    	while( l <= r ){ // <= 很重要！！！
    		if ( num[l] != dir[num[r]] ) return false;
    		l++;
    		r--;
    	}
    	return true;
    }
};
```
