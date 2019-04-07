---
layout: post
title:  "14. Longest Common Prefix"
date: 2019-03-29 13:32:23 -0400
categories: articles
---
Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

Example 1:
```
Input: ["flower","flow","flight"]
Output: "fl"
```
Example 2:
```
Input: ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```


```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(strs.empty())  
            return "";  
        int num = strs.size();  
        int len = strs[0].size();  
        for(int i = 0; i < len; i++)  
            for(int j = 1; j < num; j++)  {  
                if(i > strs[j].size() || strs[j][i] != strs[0][i])  
                    return strs[0].substr(0, i);  
            }  
        return strs[0];  
    }
};
```