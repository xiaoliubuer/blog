---
layout: post
title:  "555. Split Concatenated Strings"
date: 2019-08-11 16:11:00 -0400
categories: articles
---
Given a list of strings, you could concatenate these strings together into a loop, where for each string you could choose to reverse it or not. Among all the possible loops, you need to find the lexicographically biggest string after cutting the loop, which will make the looped string into a regular one.

Specifically, to find the lexicographically biggest string, you need to experience two phases:

Concatenate all the strings into a loop, where you can reverse some strings or not and connect them in the same order as given.
Cut and make one breakpoint in any place of the loop, which will make the looped string into a regular one starting from the character at the cutpoint.
And your job is to find the lexicographically biggest one among all the possible regular strings.

Example:
```
Input: "abc", "xyz"
Output: "zyxcba"
```
Explanation: 
```
You can get the looped string "-abcxyz-", "-abczyx-", "-cbaxyz-", "-cbazyx-", 
where '-' represents the looped status. 
The answer string came from the fourth looped one, 
where you could cut from the middle character 'a' and get "zyxcba".
```
Note:
```
The input strings will only contain lowercase letters.
The total length of all the strings will not over 1,000.
```
```c++
class Solution {
public:
    string splitLoopedString(vector<string>& strs) {
        if(strs.empty()) return "";
        string s, res ="a";
        int n = strs.size(), cur = 0;
        for(auto const &str:strs){
            auto t = string(str.rbegin(), str.rend());
            s+=str > t? str:t;
        }
        for(int i=0; i<n; ++i){
            auto t1 = strs[i], t2 = string(t1.rbegin(), t1.rend());
            auto mid = s.substr(cur+t1.size()) + s.substr(0,cur);
            for(int j=0; j<strs[i].size(); ++j){
                if(t1[j]>=res[0]) res = max(res, t1.substr(j)+mid+t1.substr(0,j));
                if(t2[j]>=res[0]) res = max(res, t2.substr(j)+mid+t2.substr(0,j));
            }
            cur+=strs[i].size();
            
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    string splitLoopedString(vector<string>& strs) {
        vector<string>tmp;
        for(auto str:strs)
        {
            string clone = str;
            reverse(clone.begin(),clone.end());
            if(str.compare(clone)>0)
                tmp.push_back(str);
            else
                tmp.push_back(clone);
        }        
        string res;
        for(int i = 0;i<tmp.size();i++)
        {
            string str = tmp[i];
            string rev = tmp[i];
            reverse(rev.begin(),rev.end());
            vector<string> candidates = {str,rev};
            for(auto candidate:candidates)
            {
                for(int k = 0;k<candidate.size();k++)
                {
                    string t = candidate.substr(k);
                    for (int j = i + 1; j < tmp.size(); j++)t+=tmp[j];
                    for (int j = 0; j < i; j++)t+=tmp[j];
                    t+=candidate.substr(0,k);
                    if (t.compare(res) > 0)
                        res = t;
                }
            } 
        }
        return res;
    }
};
```