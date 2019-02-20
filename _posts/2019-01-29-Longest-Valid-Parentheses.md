---
layout: post
title:  "** 32. Longest Valid Parentheses"
date: 2019-01-29 19:44:23 -0400
categories: articles
---
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

Example 1:
```
Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
```
Example 2:
```
Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"
```
# Function signature
```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        
    }
};
```
# 题意
给一个用 '(' 和 ')'组成的string，找到最长的合法的substring
# 想法
这个还是不太会诶～～
# shame answer
```c++
// 这个解法是，太神奇啦～怎么会想到这么解呢？
class Solution {
public:
    int longestValidParentheses(string s) {
		int res = 0;
		stack<int> valisStack;
		valisStack.push(-1);
		for(int i = 0; i < s.size(); i++){
			if( s[i] == '('){
				valisStack.push(i);
			}
			else{ //')'
				valisStack.pop();
				if(valisStack.empty())
					valisStack.push(i);
				else
					res = max(res, i - valisStack.top());
			}	
		}
		return res;
    }
};
```
```c++

class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.length();
        vector<int> dp(n, 0);
        int maxans = 0;
        for (int i = 1; i < n; i++) {
            if (s[i] == ')') {
                if (s[i-1] == '(') {
                    dp[i] = (i >= 2 ? dp[i-2] : 0) + 2;
                } else if (i-dp[i-1] > 0 && s[i - dp[i-1] - 1] == '(') {
                    dp[i] = dp[i-1] + ((i - dp[i-1]) >= 2 ? dp[i-dp[i-1]-2] : 0) + 2;
                }
                maxans = max(maxans, dp[i]);
            }
        }
        return maxans;
    }
};
```