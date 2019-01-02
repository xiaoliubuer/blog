---
layout: post
title:  "20. Valid Parentheses"
date: 2018-12-31 15:17:23 -0400
categories: articles
---
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

Example 1:
```
Input: "()"
Output: true
```
Example 2:
```
Input: "()[]{}"
Output: true
```
Example 3:
```
Input: "(]"
Output: false
```
Example 4:
```
Input: "([)]"
Output: false
```
Example 5:
```
Input: "{[]}"
Output: true
```

# Function signature
```c++
class Solution {
public:
    bool isValid(string s) {
        
    }
};
```
# 题意
给一个string包含那些括号的符号，判断这个string是不是合法的.
# 想法
用stack，如果碰到左边，就push到stack里，如果碰到右半部分，那么判断top是不是左半部分，如果是pop，如果不是，直接return false。最后return stack.empty().
1. Corner case
```c++
if (s.empty()) return false;
```
2. Init stack
```c++
stack<char> buff;
```
3. Algorithm
```c++
for (int i = 0; i < s.size(); i++) {
	char curr = s[i];
	if ( curr == '(' || curr == '[' || curr == '{' )
		buff.push(curr);
	else{
		if ( buff.empty() ) return false;
		if ( curr == ')' && buff.top() != '(') return false;
		if ( curr == ']' && buff.top() != '[') return false;
		if ( curr == '}' && buff.top() != '{') return false;
	buff.pop();
	}
}
return buff.empty();
```
# 参考答案
```c++
class Solution {
public:
    bool isValid(string s) {
        if (s.empty()) return true;
        stack<char> buff;
        for (int i = 0; i < s.size(); i++) {
            char curr = s[i];
            if ( curr == '(' || curr == '[' || curr == '{' )
                buff.push(curr);
            else{
                if ( buff.empty() ) return false;
                if ( curr == ')' && buff.top() != '(') return false;
                if ( curr == ']' && buff.top() != '[') return false;
                if ( curr == '}' && buff.top() != '{') return false;
                buff.pop();
            }
        }
        return buff.empty();
    }
};
```