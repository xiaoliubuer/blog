---
layout: post
title:  "316. Remove Duplicate Letters"
date: 2019-07-30 08:47:00 -0400
categories: articles
---

Given a string which contains only lowercase letters, remove duplicate letters so that every letter appear once and only once. You must make sure your result is the smallest in lexicographical order among all possible results.

Example 1:
```
Input: "bcabc"
Output: "abc"
```
Example 2:
```
Input: "cbacdcbc"
Output: "acdb"
```
```c++
string removeDuplicateLetters(string s) {
    vector<int> cand(256, 0);
    vector<bool> visited(256, false);
    for (char c : s)
        cand[c]++;
    string result = "0";
    for (char c : s) {
        cand[c]--;
        if (visited[c]) continue;
        while (c < result.back() && cand[result.back()]) {
            visited[result.back()] = false;
            result.pop_back();
        }
        result += c;
        visited[c] = true;
    }
    return result.substr(1);
}
```

```c++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        int count[26] = {0};
        vector<bool> included(26, false);
        
        vector<char> result;
        
        int i;
        for(i = 0; i < s.length(); i++) {
            count[s[i] - 'a']++;
        }
        
        for(i = 0; i < s.length(); i++) {
            int key = s[i] - 'a';
            count[key]--;
            if(included[key]) {
                continue;
            }
            
            while(!result.empty()) {
                char c = result.back();
                if(count[c - 'a'] == 0 || c < s[i]) {
                    break;
                }
                result.pop_back();
                included[c - 'a'] = false;
            }
            result.push_back(s[i]);
            included[key] = true;
        }
        
        
        return string(result.begin(), result.end());
    }
};
```
```c++
class Solution {
public:
    string removeDuplicateLetters(string s) {
        vector <int> hash(128,0);
        for (int i=0;i<s.size();i++){
            hash[s[i]]++;
        }
        vector<bool> visited(128,false);
        string res="";
        for (int i=0;i<s.size();i++){
            while (!visited[s[i]] && s[i]<res.back() && hash[res.back()]>0){
                visited[res.back()]=false;
                res.pop_back();
            }
            if(!visited[s[i]]){
                res+=string(1,s[i]);
                visited[s[i]]=true;
            }
            hash[s[i]]--;
        }
        return res;
    }
};
```
