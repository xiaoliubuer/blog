---
layout: post
title:  "179. Largest Number"
date: 2019-01-24 21:18:23 -0400
categories: articles
---
Given a list of non negative integers, arrange them such that they form the largest number.

Example 1:

Input: [10,2]
Output: "210"
Example 2:

Input: [3,30,34,5,9]
Output: "9534330"
Note: The result may be very large, so you need to return a string instead of an integer.
# Function signature
```c++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        
    }
};
```
# 题意
给一个list的数字，把它们组成一个最大的数。
# 想法
哇塞，很有意思的一道题啊～～
怎么判断他是最大的呢？？都转成string，然后排序？
# 尝试解解
```c++
//Accepted!!
class Solution {
public:
    string largestNumber(vector<int>& nums) {
    	vector<string> new_nums;
    	for(int i : nums){
    		new_nums.push_back(to_string(i));
    	}
    	sort(new_nums.begin(), new_nums.end(),[](string& A, string& B){
                return A + B > B + A; //这个地方太厉害了！！
        });
    	string res;
    	for(string i : new_nums){
            if ( res.size() > 0 && i == res && i == "0") continue; // 这个地方也很厉害！！需要注意！！
            res += i;
        }
    	return res;
    }
};
```