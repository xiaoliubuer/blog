---
layout: post
title:  "187. Repeated DNA Sequences"
date: 2019-01-27 16:26:23 -0400
categories: articles
---
All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.

Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

Example:
```
Input: s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"

Output: ["AAAAACCCCC", "CCCCCAAAAA"]
```
# Function signature
```c++
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
        
    }
};
```
# 题意
写一个算法找到在s中有10个char的substring出现过repeat
# 想法
想到的只能用个set，extra space。
# 尝试解解
```c++
// Accepted!!
class Solution {
public:
    vector<string> findRepeatedDnaSequences(string s) {
    	vector<string> res;
    	if ( s.size() < 10 ) return res;
    	unordered_map<string, int>  buff;
    	for ( int i = 0; i + 9 < s.size(); i++ ){
    		string sub_str = s.substr(i, 10);
    		if ( buff[sub_str] == 1 ){
    			res.push_back(sub_str);
    			buff[sub_str] = 2;
    		}
    		else if ( buff[sub_str] == 0) // 这个地方要小心。
    			buff[sub_str] = 1;
    	}
        return res;
    }
};
```