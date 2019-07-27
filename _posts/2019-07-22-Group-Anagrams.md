---
layout: post
title:  "49. Group Anagrams"
date: 2019-07-22 22:52:00 -0400
categories: articles
---
Given an array of strings, group anagrams together.

Example:
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
Note:
```
All inputs will be in lowercase.
The order of your output does not matter.
```
```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> res;
        if (strs.size() == 0) return res;
        unordered_map<string, int> map;
        for (string i:strs){
            string temp = i;
            sort(i.begin(), i.end());
            if ( map.find(i) == map.end()){
                vector<string> row;
                row.push_back(temp);
                res.push_back(row);
                map.insert({i, res.size()});
            }
            else{
                res[map[i] - 1].push_back(temp);
            }
        }
        return res;
    }
};
```