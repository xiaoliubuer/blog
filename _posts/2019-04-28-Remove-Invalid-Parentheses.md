---
layout: post
title:  "301. Remove Invalid Parentheses"
date: 2019-04-28 12:53:00 -0400
categories: articles
---
Remove the minimum number of invalid parentheses in order to make the input string valid. Return all possible results.

Note: The input string may contain letters other than the parentheses ( and ).

Example 1:
```
Input: "()())()"
Output: ["()()()", "(())()"]
```
Example 2:
```
Input: "(a)())()"
Output: ["(a)()()", "(a())()"]
```
Example 3:
```
Input: ")("
Output: [""]
```
# Function signature
```c++
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        
    }
};
```
# 题意
删除最少的'('或')'使得字符串合法。
# 想法
如何检测合法。
如何判断是最少。
如何删，以怎样的顺序和组合删会是最好的方法。

```c++
class Solution {
private:
    bool isValid(string s) {
        int sum = 0;
        for(char &c : s) {
            if(c == '(') ++sum;
            else if(c == ')') --sum;
            if(sum < 0) return false;
        }
        return sum == 0;
    }
public:
    vector<string> removeInvalidParentheses(string s) {
        int num1 = 0, num2 = 0;
        for(char &c : s) {
            num1 += c == '(';
            if (num1 == 0) {
                num2 += c == ')';
            } else {
                num1 -= c == ')';
            }
        }
        vector<string> ret;
        dfs(s, 0, num1, num2, ret);
        return ret;
    }
    void dfs(string s, int beg, int num1, int num2, vector<string> &ret) {
        if(num1 == 0 && num2 == 0) {
            if(isValid(s))
                ret.push_back(s);
        } else {
            for(int i = beg; i < s.size(); ++i) {
                string tmp = s;
                if(num2 == 0 && num1 > 0 && tmp[i] == '(') {
                    if(i == beg || tmp[i] != tmp[i - 1]) { // if previous is '(', which means the same sequence has been tried, we do not have to try again. 
                        tmp.erase(i, 1);
                        dfs(tmp, i, num1 - 1, num2, ret);
                    }
                }
                if(num2 > 0 && tmp[i] == ')') {
                    if(i == beg || tmp[i] != tmp[i - 1]) { // the same as the case for '('
                        tmp.erase(i, 1);
                        dfs(tmp, i, num1, num2 - 1, ret);
                    }
                }
            }
        }
    }
};
```
# Shame answer
```c++
class Solution {
public:
    vector<string> removeInvalidParentheses(string s) {
        unordered_set<string> result;
        int left_removed = 0;
        int right_removed = 0;
        for(auto c : s) { //先搞清楚有几左括号和右括号各多出来多少个。
            if(c == '(') {
                ++left_removed;
            }
            if(c == ')') {
                if(left_removed != 0) {
                    --left_removed;
                }
                else {
                    ++right_removed;
                }
            }
        }
        helper(s, 0, left_removed, right_removed, 0, "", result);
        return vector<string>(result.begin(), result.end());
    }
private:
    void helper(string s, int index, int left_removed, int right_removed, int pair, string path, unordered_set<string>& result) {
        if(index == s.size()) {
            if(left_removed == 0 && right_removed == 0 && pair == 0) {
                result.insert(path);
            }
            return;
        }
        if(s[index] != '(' && s[index] != ')') {
            helper(s, index + 1, left_removed, right_removed, pair, path + s[index], result);
        }
        else {
            if(s[index] == '(') {
                if(left_removed > 0) {
                    helper(s, index + 1, left_removed - 1, right_removed, pair, path, result);
                }
                helper(s, index + 1, left_removed, right_removed, pair + 1, path + s[index], result);
            }
            if(s[index] == ')') {
                if(right_removed > 0) {
                    helper(s, index + 1, left_removed, right_removed - 1, pair, path, result);
                }
                if(pair > 0) {
                    helper(s, index + 1, left_removed, right_removed, pair - 1, path + s[index], result);
                }
            }



        }
    }
};
```
