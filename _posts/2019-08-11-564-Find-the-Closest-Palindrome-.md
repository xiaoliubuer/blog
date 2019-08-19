---
layout: post
title:  "564. Find the Closest Palindrome"
date: 2019-08-11 17:06:00 -0400
categories: articles
---
Given an integer n, find the closest integer (not including itself), which is a palindrome.

The 'closest' is defined as absolute difference minimized between two integers.

Example 1:
```
Input: "123"
Output: "121"
```
Note:
```
The input n is a positive integer represented by string, whose length will not exceed 18.
If there is a tie, return the smaller one as answer.
```
```c++
class Solution {
public:
    string nearestPalindromic(string n) {
        function<long(long, int)> mirror = [&n](long mid, int d) {
            string prefix = to_string(mid + d);
            return stol(prefix + string(prefix.rbegin() + (n.size() % 2), prefix.rend()));
        };
        
        long mid = stol(n.substr(0, (n.size() + 1) / 2));
        set<long> candidates = { pow(10, n.size() - 1) - 1, mirror(mid, -1), mirror(mid, 0), mirror(mid, 1), pow(10, n.size()) + 1 };

        long num = stol(n);
        candidates.erase(num);
        return to_string(*min_element(candidates.begin(), candidates.end(), [num](long a, long b) { return abs(num - a) < abs(num - b); }));
    }
};
```
```c++
class Solution {
public:
    string nearestPalindromic(string n) {
        int len = n.size();
        vector<long> candidates;
        candidates.push_back(long(pow(10, len) + 1));
        candidates.push_back(long(pow(10, len - 1) - 1));
        
        long prefix = stol(n.substr(0, (len+1)/2));
        for (int i = -1; i <= 1; i++) {
            string p = to_string(prefix + i);
            string pp= p + string(p.rbegin() + (len & 1), p.rend());
            candidates.push_back(stol(pp));
        }
        
        string res = "";
        long num = stol(n);
        for (auto c : candidates) {
            if (c == num) continue;
            if (res == "" || abs(num - c) < abs(num - stol(res))) {
                res = to_string(c);
            } else if (abs(num - c) == abs(num - stol(res)) && stol(res) > c) {
                res = to_string(c);
            }
        }
        
        return res;
    }
};
```