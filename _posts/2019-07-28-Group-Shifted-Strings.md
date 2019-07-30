---
layout: post
title:  "249. Group Shifted Strings"
date: 2019-07-28 15:58:00 -0400
categories: articles
---
Given a string, we can "shift" each of its letter to its successive letter, for example: "abc" -> "bcd". We can keep "shifting" which forms the sequence:

"abc" -> "bcd" -> ... -> "xyz"
Given a list of strings which contains only lowercase alphabets, group all strings that belong to the same shifting sequence.

Example:

Input: ["abc", "bcd", "acef", "xyz", "az", "ba", "a", "z"],
Output: 
[
  ["abc","bcd","xyz"],
  ["az","ba"],
  ["acef"],
  ["a","z"]
]
```c++
class Solution {
public:
    vector<vector<string>> groupStrings(vector<string>& strings) {
        unordered_map<string, vector<string> > mp;
        for (string  s : strings)
            mp[shift(s)].push_back(s);
        vector<vector<string> > groups;
        for (auto m : mp) {
            vector<string> group = m.second;
            sort(group.begin(), group.end());
            groups.push_back(group);
        }
        return groups;
    }
private:
    string shift(string& s) {
        string t;
        int n = s.length();
        for (int i = 1; i < n; i++) {
            int diff = s[i] - s[i - 1];
            if (diff < 0) diff += 26;
            t += 'a' + diff + ',';
        }
        return t;
    }
};
```
```c++
class Solution {
public:
    vector<vector<string>> groupStrings(vector<string>& strings) {
        if(strings.size()==0)
            return {};
        vector<vector<string>> ans;
        unordered_map<string, vector<string>> help;
        for(auto it: strings){
            string key = helper(it);
            help[key].push_back(it);
        }
        
        for(auto it: help){
            ans.push_back(it.second);
        }
        
        return ans;
    }
    
    string helper(string key){
        string ans;
        for(int i=1;i<key.size();i+=1){
            int diff = key[i-1] - key[i];
            if(diff<0)
                diff+=26;
            
            ans+= char(diff);
        }
        return ans;
    }
};
```