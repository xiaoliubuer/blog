---
layout: post
title:  "139. Word Break"
date: 2019-05-12 16:39:00 -0400
categories: articles
---

Given a non-empty string s and a dictionary wordDict containing a list of non-empty words, determine if s can be segmented into a space-separated sequence of one or more dictionary words.

Note:

The same word in the dictionary may be reused multiple times in the segmentation.
You may assume the dictionary does not contain duplicate words.
Example 1:
```
Input: s = "leetcode", wordDict = ["leet", "code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```
Example 2:
```
Input: s = "applepenapple", wordDict = ["apple", "pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
             Note that you are allowed to reuse a dictionary word.
```
Example 3:
```
Input: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
Output: false
```
```c++
// DP solution, answer
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        vector<int> dp(s.size()+1, 0);
        dp[0] = 1;
        for (int i = 0; i < s.size(); i++) {
            for (auto &w : wordDict) {
                int idx = i-(int)w.size()+1;
                if (idx >= 0 && dp[idx] == 1) {
                    string temp = s.substr(idx, w.size());
                    if (temp == w) dp[i+1] = 1;
                }
            }
        }
        return dp[dp.size()-1];
    }
};
```

```c++
class Solution {
public:
    bool helper(string s, unordered_set<string>& check, unordered_map<string, bool>& visited){
        if ( visited.count(s) > 0 ) return visited[s];
        if ( check.count(s) > 0 ) return visited[s] = true;
        for ( int i = 1; i < s.size(); i++) {
            if ( check.count( s.substr(0, i)) > 0 && helper( s.substr(i), check, visited)) 
                return visited[s] = true;
        }
        return visited[s] = false;
    }
    bool wordBreak(string s, vector<string>& wordDict) {
        unordered_set<string> check(wordDict.begin(), wordDict.end());
        unordered_map<string, bool> visited;
        return helper(s, check, visited);
    }
};
```