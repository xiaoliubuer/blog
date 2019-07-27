---
layout: post
title:  "224. Basic Calculator"
date: 2019-07-27 19:26:00 -0400
categories: articles
---
Implement a basic calculator to evaluate a simple expression string.

The expression string may contain open ( and closing parentheses ), the plus + or minus sign -, non-negative integers and empty spaces .

Example 1:
```
Input: "1 + 1"
Output: 2
```
Example 2:
```
Input: " 2-1 + 2 "
Output: 3
```
Example 3:
```
Input: "(1+(4+5+2)-3)+(6+8)"
Output: 23
```
Note:
You may assume that the given expression is always valid.
Do not use the eval built-in library function.

```c++
class Solution {
public:
    int calculate(string s) {
        if (s.empty()) return 0;
        const int n = s.size();
        stack<int> stack;
        int operand = 0; int result = 0; int sign = 1;
        for (int i=0; i<n; i++) {
            char ch = s[i];
            if (isDigit(ch)) {
                operand = 10 * operand + (ch-'0');
            } else if (ch == '+') {
                result += sign*operand;
                sign = +1; operand = 0;
            } else if (ch == '-') {
                result += sign*operand;
                sign = -1; operand = 0;
            } else if (ch == '(') {
                stack.push(result); stack.push(sign);
                sign = 1; result = 0;
            } else if (ch == ')') {
                result += sign*operand;
                if (!stack.empty()) {
                    result *= stack.top();
                    stack.pop();
                }
                if (!stack.empty()) {
                    result += stack.top();
                    stack.pop();
                }
                operand = 0;
            }
        }
        return result + (sign * operand);
    }
    bool isDigit(char ch) {
        return ch>='0' && ch<='9';
    }
};
```