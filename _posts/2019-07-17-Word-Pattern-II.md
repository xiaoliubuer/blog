---
layout: post
title:  "291. Word Pattern II"
date: 2019-07-16 20:48:00 -0400
categories: articles
---

Given a pattern and a string str, find if str follows the same pattern.

Here follow means a full match, such that there is a bijection between a letter in pattern and a non-empty substring in str.

Example 1:
```
Input: pattern = "abab", str = "redblueredblue"
Output: true
```
Example 2:
```
Input: pattern = pattern = "aaaa", str = "asdasdasdasd"
Output: true
```
Example 3:
```
Input: pattern = "aabb", str = "xyzabcxzyabc"
Output: false
```
Notes:
You may assume both pattern and str contains only lowercase letters.


```c++
class Solution {
public:
    bool wordPatternMatch(string pattern, string str) {
        return match(pattern, str);
    }
    
    bool match(string_view pattern, string_view str) {
        if (pattern.empty() && str.empty()) {
            return true;
        }
        if (pattern.empty() || pattern.size() > str.size()) {
            return false;
        }
        char unit = pattern[0];
        if (auto it = map_.find(unit); it != map_.end()) {
            auto expansion = it->second;
            if (startsWith(str, expansion)) {
                return match(pattern.substr(1), str.substr(expansion.size()));
            } else {
                return false;
            }
        } 
        for (size_t len = str.size() - pattern.size() + 1; len > 0; len--) {
            auto expansion = str.substr(0, len);
            if (expansions_.count(expansion)) {
                continue;
            }
            setupMap(unit, expansion);
            bool matched = match(pattern.substr(1), str.substr(len));
            if (matched) {
                return true;
            }
            tearDownMap(unit);
        }
        return false;
    }
    
    bool startsWith(string_view body, string_view prefix) {
        return body.substr(0, prefix.size()) == prefix;
    }
                
    void setupMap(char unit, string_view expansion) {
        expansions_.insert(expansion);
        map_[unit] = expansion;  
    }
                
    void tearDownMap(char unit) {
        expansions_.erase(map_[unit]);
        map_.erase(unit);
    }
    
private:
    unordered_map<char, string_view> map_;
    unordered_set<string_view> expansions_;
};
```