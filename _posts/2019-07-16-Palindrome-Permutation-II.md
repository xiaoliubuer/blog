---
layout: post
title:  "267. Palindrome Permutation II"
date: 2019-07-16 19:15:00 -0400
categories: articles
---	
Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.

Example 1:

Input: "aabb"
Output: ["abba", "baab"]
Example 2:

Input: "abc"
Output: []

```c++
// Answer
class Solution {
public:
    vector<string> generatePalindromes(string s) {
        unordered_set<string> res;
        unordered_map<char, int> m;
        string t = "", mid = "";
        for (auto a : s) ++m[a];
        for (auto it : m) {
            if (it.second % 2 == 1) mid += it.first;
            t += string(it.second / 2, it.first);
            if (mid.size() > 1) return {};
        }
        helper(t, 0, mid, res);
        return vector<string>(res.begin(), res.end());
    }
    
    void helper(string &t, int start, string mid, unordered_set<string> &res) {
        if(start >= t.size())
        {
            res.insert(t+mid+string(t.rbegin(), t.rend()));
            return;
        }
        for(int i = start; i < t.size(); ++i)
        {
            if(i != start && t[i] == t[start]) continue;
            swap(t[i], t[start]);
            helper(t, start+1, mid, res);
            swap(t[i], t[start]);
        }
    }
};
```

```c++
// Good, but Timeout
class Solution {
public:
    vector<string> generatePalindromes(string s) {
        vector<string> res;
        vector<bool> visited(s.size(), false);
        unordered_set<string> found;
        helper(s, "", res, visited, found);
        return res;
    }
    
    void helper(string s, string temp, vector<string>& res, vector<bool>& visited, unordered_set<string>& found){
        if ( temp.size() == s.size() ) {
            if (verify(temp) && !found.count(temp)) {
                res.push_back(temp);
                found.insert(temp);
                return;
            }
        }
        
        for ( int i = 0; i < s.size(); i++ ) {
            string curr = temp;
            if ( !visited[i] ){
                curr.push_back(s[i]);
                visited[i] = true;
                helper(s, curr, res, visited, found);
                visited[i] = false;
            }
        }
    }
    
    bool verify(string s){
        int l = 0, r = s.size() - 1;
        while ( l < r ) {
            if ( s[l] != s[r]) return false;
            l++;
            r--;
        }
        return true;
    }
};
```