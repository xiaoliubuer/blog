---
layout: post
title:  "524. Longest Word in Dictionary through Deleting"
date: 2019-08-07 20:42:00 -0400
categories: articles
---
Given a string and a string dictionary, find the longest string in the dictionary that can be formed by deleting some characters of the given string. If there are more than one possible results, return the longest word with the smallest lexicographical order. If there is no possible result, return the empty string.

Example 1:
```
Input:
s = "abpcplea", d = ["ale","apple","monkey","plea"]

Output: 
"apple"
```
Example 2:
```
Input:
s = "abpcplea", d = ["a","b","c"]

Output: 
"a"
```
Note:
```
All the strings in the input will only contain lower-case letters.
The size of the dictionary won't exceed 1,000.
The length of all the strings in the input won't exceed 1,000.
```
```c++
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        string res = "";
        for (string str : d) {
            int i = 0;
            for (char c : s) {
                if (i < str.size() && c == str[i]) ++i;
            }
            if (i == str.size() && str.size() >= res.size()) {
                if (str.size() > res.size() || str < res) {
                    res = str;
                }
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
	bool isSubsequence(string str, string s){
		int j = 0;
		for (int i = 0; i < s.size() && j < str.size(); ++i){
			if (str[j] == s[i]) j++;
		}
		return j == str.size();
	}
    string findLongestWord(string s, vector<string>& d) {
       string max_str = "";
       if (s.size() == 0 || d.size() == 0) return max_str;

       for (auto str : d){
       		if (isSubsequence(str, s)){
       			if ( str.size() > max_str.size() || ( str.size() == max_str.size() && str.compare(max_str) < 0)){
       				max_str = str;
       			}
       		}
       }
       return max_str;
    }
};
```
```c++
class Solution {
public:
    string findLongestWord(string s, vector<string>& d) {
        string ans = "";
        for (auto str : d) {
            int i = 0;
            int j = 0;
            while (i < s.size() && j < str.size()) {
                if (s[i] == str[j]) {
                    j++;
                }
                i++;
            }
            
            if (j >= str.size() && 
                (str.size() > ans.size() || 
                 (str.size() == ans.size() && str < ans))) {
                ans = str;
            }
            
        }
        return ans;
    }
};
```