---
layout: post
title:  "131. Palindrome Partitioning"
date: 2019-06-25 21:12:00 -0400
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
```c++
class Solution {
public:
    void helper(int idx, vector<vector<string>>& res, vector<string> temp, string s){
        if ( idx == s.size() ) {
            res.push_back(temp);
            return;
        }
        
        for ( int i = idx; i < s.size(); i++ ) {
            int left = idx, right = i;
            while ( left < right && s[left] == s[right] ) left++, right--;
            if ( left >= right ) {
                temp.push_back(s.substr(idx, i - idx + 1)); //important!!
                helper(i + 1, res, temp, s);
                temp.pop_back(); //important
            }
        }
    }
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> temp;
        helper(0, res, temp, s);
        return res;
    }
};
```
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