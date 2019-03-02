---
layout: post
title:  "* 358. Rearrange String k Distance Apart"
date: 2019-02-25 20:15:23 -0400
categories: articles
---
Given a non-empty string s and an integer k, rearrange the string such that the same characters are at least distance k from each other.

All input strings are given in lowercase letters. If it is not possible to rearrange the string, return an empty string "".

Example 1:
```
Input: s = "aabbcc", k = 3
Output: "abcabc" 
Explanation: The same letters are at least distance 3 from each other.
```
Example 2:
```
Input: s = "aaabc", k = 3
Output: "" 
Explanation: It is not possible to rearrange the string.
```
Example 3:
```
Input: s = "aaadbbcc", k = 2
Output: "abacabcd"
Explanation: The same letters are at least distance 2 from each other.
```
# Function signature
class Solution {
public:
    string rearrangeString(string s, int k) {
        
    }
};

# 题意
给一个string s，和一个k，重新排列string，使同样的char之间至少相距k。如果无法完成，返回“”

# 考点
贪心？
没想到要考什么点。

# 问题点
还是没有想到如何下手的切入点

# Shame answer
```c++
class Solution {
public:
    using Pair = pair<int, char>;
    string rearrangeString(string s, int K) {
        if (K == 0) return s; // no processing needed
        unordered_map<char, int> m;
        for (char c : s) m[c]++;// 记录每个字符出现的次数
        priority_queue<Pair> pq; // queue，最小堆，头顶最小
        for (auto p : m) pq.emplace(p.second, p.first); // (count, char)

        string res;
        while (!pq.empty()) {
            vector<Pair> cache; // store characters we used this round each round, we add K characters to res
            for (int i = 0; i < K; i++) {
                if (pq.empty()) return ""; // no available characters
                Pair temp = pq.top(); pq.pop(); // (count, char)
                res.push_back(temp.second);
                if (res.size() == s.size()) return res; // done!
                if (--temp.first > 0) cache.push_back(temp);
            }
            for (Pair& p : cache) pq.push(p);
        }
        return ""; // handle empty string
    }
};
```

# 感想
非常巧妙，这道题成立有几个前提需要捋清楚，才能下手。重点就在这个while循环里面。
1. 能够保证出现最多的那个char在都处于至少相隔最多的那个区间里，就能保证。
