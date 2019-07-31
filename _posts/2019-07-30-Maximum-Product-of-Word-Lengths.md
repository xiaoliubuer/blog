---
layout: post
title:  "318. Maximum Product of Word Lengths"
date: 2019-07-30 19:24:00 -0400
categories: articles
---

Given a string array words, find the maximum value of length(word[i]) * length(word[j]) where the two words do not share common letters. You may assume that each word will contain only lower case letters. If no such two words exist, return 0.

Example 1:
```
Input: ["abcw","baz","foo","bar","xtfn","abcdef"]
Output: 16 
Explanation: The two words can be "abcw", "xtfn".
```
Example 2:
```
Input: ["a","ab","abc","d","cd","bcd","abcd"]
Output: 4 
Explanation: The two words can be "ab", "cd".
```
Example 3:
```
Input: ["a","aa","aaa","aaaa"]
Output: 0 
Explanation: No such pair of words.
```

```c++
class Solution {
public:
    int maxProduct(vector<string>& words) {
        unordered_map<bitset<26>, int> max_map;
        for (auto &w: words) {
            bitset<26>bs;
            for (auto c: w) {
                bs.set(c - 'a');
            }
            max_map[bs] = max(max_map[bs], (int)w.size());
        }
        int maxprod = 0;
        for (auto m: max_map) {
            for (auto n: max_map) {
                if ((m.first & n.first) == 0) {
                    maxprod = max(maxprod, m.second * n.second);
                }
            }
        }
        return maxprod;
        
    }
};
```
```c++
class Solution {
public:
    
    int maxProduct(vector<string>& words) {
        int len = words.size();
        vector<int> mask(len);
        
        for(int i = 0; i < len; i++) {
            int sz = words[i].size();
            for(int j = 0; j < sz; j++) {
                mask[i] |= (1 << (words[i][j] - 'a'));
            }
        }
        
        int ans = 0;
        for(int i = 0; i < len; i++) {
            for(int j = 0; j < i; j++) {
                if(mask[i] & mask[j]) continue;
                int p = words[i].size()*words[j].size();
                if(p > ans) ans = p;
            }
        }
        return ans;
    }
};
```

```c++
bool cmp_str(const string &s1, const string &s2) {
    return s1.length() < s2.length();
}

class Solution {
public:
    int maxProduct(vector<string>& words) {
        if (words.empty()) return 0;
        
        sort(words.begin(), words.end(), cmp_str);
        int size = words.size();
        int word_int[size];
        memset(word_int, 0, sizeof(word_int));
       
        size_t max_product = 0;
        for (int i=0; i<size; i++) {
            for (auto &c : words[i]) word_int[i] |= (1<<(c-'a'));
            
            for (int j=i-1; j>=0; j--) {
                if (!(word_int[j] & word_int[i])) {
                    max_product = max(max_product, words[i].length()*words[j].length());
                    break; // pruning
                }
            }
        }
       
        return max_product;
    }
};
```
```c++
class Solution {
public:
int maxProduct(vector<string>& words) {
    vector<int> mask(words.size());
    int result = 0;
    for (int i=0; i<words.size(); ++i) {
        for (char c : words[i])
            mask[i] |= 1 << (c - 'a');
        for (int j=0; j<i; ++j)
            if (!(mask[i] & mask[j]))
                result = max(result, int(words[i].size() * words[j].size()));
    }
    return result;
}
};
```