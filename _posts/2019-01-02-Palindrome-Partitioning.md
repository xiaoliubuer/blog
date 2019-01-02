---
layout: post
title:  "131. Palindrome Partitioning"
date: 2019-01-02 07:54:23 -0400
categories: articles
---
Given a string s, partition s such that every substring of the partition is a palindrome.

Return all possible palindrome partitioning of s.

Example:
```
Input: "aab"
Output:
[
  ["aa","b"],
  ["a","a","b"]
]
```
# Funciton signature
```c++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        
    }
};
```
# 题意
一个string s，分割这个s，是每个substirng都是一个回文string。返回所有的可能性。
# Thinking
How to confirm it is palindrome or not?
The bad complexity comes from the way to check it is palindrome.
# Shame answer
```c++
class Solution {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> tmp;
        getPartition(s, 0, tmp, res);
        return res;
    }
private:
    void getPartition(string& s, int idx, vector<string>& tmp, vector<vector<string>>& res){
        if(idx == s.length()){
            res.push_back(tmp);
            return;
        }
        for(int i = idx, n = s.length(); i < n; i++){
            int l = idx, r = i;
            // Check is palindrome
            while( l < r && s[l] == s[r]) l++, r--;
            if( l >= r ){
                tmp.push_back(s.substr(idx, i-idx + 1));
                getPartition(s, i + 1, tmp, res);
                tmp.pop_back();
            }
        }
    }
};
```
```c++
class Solution {
public:
    bool isPalindrome(string& s, int low, int high){
        while (low < high)
            if (s[low++] != s[high--]) return false;
        return true;
    }
    void backtrack(auto& output, auto& current, string& s, int index){
        if (index == s.size()){
            output.push_back(current);
            return;
        }

        for (int length = index; length < s.size(); ++length){
            if (isPalindrome(s, index, length)){
                current.push_back(s.substr(index, length-index+1));
                backtrack(output, current, s, length+1);
                current.pop_back();
            }
        }
    }
    vector<vector<string>> partition(string s) {
        vector<vector<string>> output;
        vector<string> current;
        backtrack(output, current, s, 0);
        return output;
    }
};
```