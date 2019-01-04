---
layout: post
title:  "* 30. Substring with Concatenation of All Words"
date: 2019-01-03 08:15:23 -0400
categories: articles
---
You are given a string, s, and a list of words, words, that are all of the same length. Find all starting indices of substring(s) in s that is a concatenation of each word in words exactly once and without any intervening characters.

Example 1:
```
Input:
  s = "barfoothefoobarman",
  words = ["foo","bar"]
Output: [0,9]
```
Explanation: Substrings starting at index 0 and 9 are "barfoor" and "foobar" respectively.
The output order does not matter, returning [9,0] is fine too.
Example 2:
```
Input:
  s = "wordgoodgoodgoodbestword",
  words = ["word","good","best","word"]
Output: []
```
# Funciton signature
```c++
class Solution {
public:
    vector<int> findSubstring(string s, vector<string>& words) {
        
    }
};
```
# 题意
一个string s和一列长度相同的words，找到所有的substring的indices包含所有的单词。
# Thinking
Really have no idea to break it down as a smaller problem.
append as all the permutation, then search from s to get the indices.
# Shame answer
```c++
vector<int> findSubstring(string S, vector<string> &L) {
        vector<int> ans;
        int n = S.size(), cnt = L.size();
        if (n <= 0 || cnt <= 0) return ans;
        // init word occurence
        unordered_map<string, int> dict;
        for (int i = 0; i < cnt; ++i) dict[L[i]]++;
        // travel all sub string combinations
        int wl = L[0].size();
        for (int i = 0; i < wl; ++i) {
            int left = i, count = 0;
            unordered_map<string, int> tdict;
            for (int j = i; j <= n - wl; j += wl) {
                string str = S.substr(j, wl);
                // a valid word, accumulate results
                if (dict.count(str)) {
                    tdict[str]++;
                    if (tdict[str] <= dict[str]) 
                        count++;
                    else {
                        // a more word, advance the window left side possiablly
                        while (tdict[str] > dict[str]) {
                            string str1 = S.substr(left, wl);
                            tdict[str1]--;
                            if (tdict[str1] < dict[str1]) count--;
                            left += wl;
                        }
                    }
                    // come to a result
                    if (count == cnt) {
                        ans.push_back(left);
                        // advance one word
                        tdict[S.substr(left, wl)]--;
                        count--;
                        left += wl;
                    }
                }
                // not a valid word, reset all vars
                else {
                    tdict.clear();
                    count = 0;
                    left = j + wl;
                }
            }
        }
        return ans;
    }
```