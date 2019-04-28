---
layout: post
title:  "* 32. Longest Valid Parentheses"
date: 2019-04-24 07:42:00 -0400
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
//Accepted!!! My answer
class Solution {
public:
    int longestValidParentheses(string s) {
        int res = 0;
        for (int i = 0; i < s.size(); i++){
            if ( s[i] == '('){
                int lc = 0;
                int j = i;
                while ( j < s.size() ){
                    if ( s[j] == '(' ) lc++;
                    else lc--;
                    if (lc == 0) res = max(res, j + 1 - i);
                    if ( lc < 0 ) break;
                    j++;
                }
                if (lc < 0) res = max(res, j-i);
            }
        }
        return res;
    }
};
```
```c++
// Faster a little bit
class Solution {
public:
    int longestValidParentheses(string s) {
        int res = 0;
        for (int i = 0; i < s.size(); i++){
            if ( s[i] == '('){
                int lc = 0;
                int j = i;
                while ( j < s.size() ){
                    if ( s[j] == '(' ) lc++;
                    else lc--;
                    if (lc == 0) res = max(res, j + 1 - i);
                    if ( lc < 0 ) break;
                    j++;
                }
                if (lc < 0) {
                    res = max(res, j-i);
                    i = j;
                }
            }
        }
        return res;
    }
};
```
```c++
// Accepted!! 2019-04-23
class Solution {
public:
    int longestValidParentheses(string s) {
        int counter = 0;
        int res = 0;
        
        for ( int i = 0; i < s.size(); i++){
            if ( s[i] == ')') continue;
            counter = 0;
            counter++;
            for ( int j = i + 1; j < s.size(); j++ ) {
                if ( s[j] == '(')
                    counter++;
                else
                    counter--;
                if ( counter == 0 )
                    res = max(res, j - i + 1);
                if ( counter < 0 ) break;
            }
        }
        return res;
    }
};
```
```c++
// Accepted! 2019-04-27
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.size();
        int res = 0;
        
        if ( n == 0 ) return res;
        vector<int> dp(n, 0);
        for (int i = 1; i < n; i++ ){
            if ( s[i] == ')') { // It's left part only has two situation, ) or (
                if ( s[i - 1] == '(') {
                    if ( i - 2 >= 0 ) {
                        dp[i] = dp[i - 2] + 2;
                    }
                    else{ 
                        dp[i] = 2;
                    }
                }
                else { // s[i] == ')'
                    if ( dp[i-1] != 0 ) {
                        if(i - 1 - dp[i-1] >= 0 && s[i - 1 - dp[i-1]] == '(') {
                            if ( i - 1 - dp[i-1] - 1 >=0 )
                                dp[i] = dp[i - 1 - dp[i-1] - 1 ] + dp[i-1]+ 2;
                            else
                                dp[i] = dp[i-1] + 2;
                        }
                        else
                            dp[i] = 0;
                    }
                }
            }  
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

```c++
// 这个解法是，太神奇啦～怎么会想到这么解呢？
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> buff;
        int res = 0;
        buff.push(-1);
        for ( int i = 0; i < s.size(); i++){
            if ( s[i] == '(')
                buff.push(i);
            else {
                buff.pop();
                if ( !buff.empty() )
                    res = max(res, i - buff.top());
                else 
                    buff.push(i);
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

```c++
class Solution {
public:
    int longestValidParentheses(string s) {
        int n = s.size();
        vector<int> dp(n, 0);
        int res = 0;
        if ( s.size() == 0 ) return 0;
        for(int i = 1; i < s.size(); i++){
            if ( s[i] == '(' )
                 dp[i] =  s[i-1] == ')' ? dp[i - 1] : 0;
            else{
                dp[i] = s[ i - 1 - dp[i-1]] == '(' ? dp[ i - 1 - dp[ i - 1]] + dp[i-1] + 2 : 0;
                res = max(res, dp[i]);
            }
        }
        return res;
    }
};
```