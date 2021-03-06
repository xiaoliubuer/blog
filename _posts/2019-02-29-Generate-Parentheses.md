---
layout: post
title:  "22. Generate Parentheses"
date: 2019-03-29 13:14:23 -0400
categories: articles
---
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

For example, given n = 3, a solution set is:
```
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```
```c++
// Accepted!! 2019-07-10
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> res;
        if ( n == 0 ) return res;
        
        helper("", 0, 0, n, res);
        return res;
    }
    
    void helper(string tmp, int l, int r, int n, vector<string>& res) {
        if (tmp.size() == n * 2) {
            res.push_back(tmp);
            return;
        }
        if ( l < n ) {
            tmp.push_back('(');
            helper(tmp, l + 1, r, n, res);
            tmp.pop_back();
        }
        
        if ( r < l ) {
            tmp.push_back(')');
            helper(tmp, l, r + 1, n, res);
            tmp.pop_back();   
        }
    }
};
```
```c++
// Accepted!!!!
class Solution {
public:

    void helper(vector<string>& res, string temp, int lp, int rp, int n) {
        if (temp.size() == n*2) {
            res.push_back(temp);
            return;
        }
        if ( lp < n )  helper(res, temp + "(", lp + 1, rp, n);
        if ( rp < n && rp < lp) helper(res, temp + ")", lp, rp + 1, n);
    }
    vector<string> generateParenthesis(int n) {
        int lp = 0, rp = 0;
        vector<string> res;
        helper(res, "", 0, 0, n);
        return res;
    } 
};
```