---
layout: post
title:  "* 22. Generate Parentheses"
date: 2018-12-31 18:15:23 -0400
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
# Funciton signature
```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        
    }
};
```
# 题意
给n对扩宽，写一个函数来生产所有可行的括号的组合

# 想法
第一眼看上来真的没啥想法。

如果输入的是n，那就意味着有n个 '(' 和 n个 ')', 那是什么意思呢？？我如何能构建这valid
# 看答案
```c++
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        helper(result, n, n, "");
        return result;
    }
    void helper(vector<string>& result, int m, int n, string temp){
        if(m == 0 && n == 0){
            result.push_back(temp);
            return;
        }
        if(m>0)
            helper(result, m-1, n, temp+'(');
        if(m<n)
            helper(result, m, n-1, temp+')');
    }
};
```
# 答案解析
创建一个result用来保存答案。创建一个helper进行递归。
具体是怎么做的呢？？