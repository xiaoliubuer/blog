---
layout: post
title:  "* 394. Decode String"
date: 2019-01-18 21:15:23 -0400
categories: articles
---
Given an encoded string, return it's decoded string.

The encoding rule is: k[encoded_string], where the encoded_string inside the square brackets is being repeated exactly k times. Note that k is guaranteed to be a positive integer.

You may assume that the input string is always valid; No extra white spaces, square brackets are well-formed, etc.

Furthermore, you may assume that the original data does not contain any digits and that digits are only for those repeat numbers, k. For example, there won't be input like 3a or 2[4].

Examples:
```
s = "3[a]2[bc]", return "aaabcbc".
s = "3[a2[c]]", return "accaccacc".
s = "2[abc]3[cd]ef", return "abcabccdcdcdef".
```
# Function signature
```c++
class Solution {
public:
    string decodeString(string s) {
    	if ( s.size() == 0) return s;
    	int i = s.size() - 1;
    	stack<char> buf;

    	string tmp;
    	while ( i > 0 ){
    		char ch = s[i];
    		if ( ch != '[') buf.push(ch);
    		else {
    			while ( buf.top() != ']'){
    				tmp += buf.top();
    				buf.pop();
    			}
    			int repeat = s[--i] - '0';
    			string sub;
    			for (int i = 0; i < repeat; ++i){
    				sub += tmp;
    			}
    			buf.push(sub);
    		}
    		i--;
    	}
        return buf.top();
    }
};
```
# 题意
给了一串编码后的string，返回它解码后的样子。
# 想法
哇，这样对字符串进行处理的操作我真的很不在行。
需要一个helper 函数，这个helper函数有两个作用。
1. 从这数组头开始扫描，扫到'[' 这个符号，然后把前面的一部分取出来转换成integer
2. 然后从后往前扫，碰到第一个 ']'，那么久把 [ 和 ] 之间的substring放入虾个循环。
# 尝试解解
```c++
// 思路对，细节还有很多！！
class Solution {
public:
	string helper(string s){
		int l = 0, m = 0, r = s.size() - 1;
		
		// Get repeat times
		int times = 0;
		while(l < s) {
			if (isdigit(s[l++])) continue;
			else 
				times = s.substr(0, l);
		}
		
		// Put substr which has [] in it to 
		stack<char> tmp;
		m = l+1;
		string res, ret;
		while (m < s){
			if ( tmp.empty() && s[l] == ']'){
				ret = helper(s.substr(l+1, m - l -1));
			}
			if ( s[m] == '[')
				tmp.push(s[m]);
			else if (s[m] == ']' && !tmp.empty())
				tmp.pop();
			m++;
		}
		// repeat 
		for (int i = 0; i < times; ++i){
			res += ret;
		}

		res += helper(s.substr(m+1, r - m - 1));
		// append
		return res
	}
    string decodeString(string s) {
    	if(s.size() == 0) return s;
    	return helper(s);
    }
};
```

```c++
class Solution {
public:
    string decodeString(string s) {
    	if ( s.size() == 0) return s;
    	int i = s.size() - 1;
    	stack<string> buf;
        string res;
    	
    	while ( i >= 0 ){
    		string ch = s.substr(i,1);
    		if ( ch != "[") buf.push(ch);
    		else {
                string tmp;
    			while ( buf.top() != "]"){
    				tmp += buf.top();
    				buf.pop();
    			}
                buf.pop();
                i--;
    			int repeat = stoi(s.substr(i,1));
    			string sub;
    			for (int i = 0; i < repeat; ++i){
    				sub += tmp;
    			}
    			buf.push(sub);
    		}
    		i--;
    	}
        while (!buf.empty()){
            res += buf.top();
            buf.pop();
        }
        return res;
    }
};
```