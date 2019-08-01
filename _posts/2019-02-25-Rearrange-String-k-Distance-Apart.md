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
```c++
class Solution {
public:
    string rearrangeString(string str, int k) {
        if(k == 0) return str;
        int length = (int)str.size(); 
        
        string res;
        unordered_map<char, int> dict;
        priority_queue<pair<int, char>> pq;
        
        for(char ch : str) dict[ch]++;
        for(auto it = dict.begin(); it != dict.end(); it++){
            pq.push(make_pair(it->second, it->first));
        }
        
        while(!pq.empty()){
            vector<pair<int, char>> cache; //store used char during one while loop
            int count = min(k, length); //count: how many steps in a while loop
            for(int i = 0; i < count; i++){
                if(pq.empty()) return "";
                auto tmp = pq.top();
                pq.pop();
                res.push_back(tmp.second);
                if(--tmp.first > 0) cache.push_back(tmp);
                length--;
            }
            for(auto p : cache) pq.push(p);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    /*
    One vector save each char's count
    One vector save each char's left-most possible position
    From left to right, try to fill in each pos with a valid char:
      among the chars with left-most possible position < cur_pos,
      select the chars that has the largest count.
      If such char doesn't exist, then it means all chars cannot be put into this pos, return ""
    */
    string rearrangeString(string s, int k) {
        vector<int> count(26, 0);
        vector<int> index(26, 0);
        for (int i = 0; i < s.size(); i++) {
            count[s[i] - 'a']++;
        }
        string res(s.size(), ' ');
        for (int i = 0; i < s.size(); i++) {
            int valid_char = findNextValid(count, index, i);
            if (valid_char == -1) return "";
            res[i] = 'a' + valid_char;
            index[valid_char] += k;
            count[valid_char] --;
        }
        return res;
    }
    
    int findNextValid(vector<int>& count, vector<int>& index, int pos) {
        int valid_char = -1;
        int max_count = 0;
        for (int i = 0; i < 26; i++) {
            if (count[i] > max_count && index[i] <= pos) {
                valid_char = i;
                max_count = count[i];
            }
        }
        return valid_char;
    } 
};
```
# 感想
非常巧妙，这道题成立有几个前提需要捋清楚，才能下手。重点就在这个while循环里面。
1. 能够保证出现最多的那个char在都处于至少相隔最多的那个区间里，就能保证。
