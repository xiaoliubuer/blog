---
layout: post
title:  "692. Top K Frequent Words"
date: 2019-08-18 18:08:00 -0400
categories: articles
---
Given a non-empty list of words, return the k most frequent elements.

Your answer should be sorted by frequency from highest to lowest. If two words have the same frequency, then the word with the lower alphabetical order comes first.

Example 1:
```
Input: ["i", "love", "leetcode", "i", "love", "coding"], k = 2
Output: ["i", "love"]
```
Explanation: 
```
"i" and "love" are the two most frequent words.
    Note that "i" comes before "love" due to a lower alphabetical order.
```
Example 2:
```
Input: ["the", "day", "is", "sunny", "the", "the", "the", "sunny", "is", "is"], k = 4
Output: ["the", "is", "sunny", "day"]
```
Explanation:
```
 "the", "is", "sunny" and "day" are the four most frequent words,
    with the number of occurrence being 4, 3, 2 and 1 respectively.
```
Note:
```
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Input words contain only lowercase letters.
```
Follow up:
```
Try to solve it in O(n log k) time and O(n) extra space.
```
```c++
class Solution {
public:
    struct Comp {
        bool operator()(pair<string, int> &lhs, pair<string, int> &rhs) {
            if (lhs.second > rhs.second) {
                return true;
            }
            else if (lhs.second < rhs.second) {
                return false;
            }
            return lhs.first < rhs.first;
        }
    };
    
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> m;
        for (auto &word: words) {
            ++m[word];
        }
        priority_queue<pair<string, int>, vector<pair<string,int>>, Comp> pq;
        auto it = m.begin();
        for (int i = 0; i < m.size(); ++i, ++it) {
            pq.push(*it);
            if (pq.size() > k) {
                pq.pop();
            }
        }
        vector<string> result;
        while (!pq.empty()) {
            result.push_back(pq.top().first);
            pq.pop();
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```
```c++
class Solution {
public:
    class comparator{
    public:
        bool operator()(const pair<string, int>& var1, const pair<string, int>& var2) {
            if (var1.second < var2.second) return true;
            else if (var1.second > var2.second) return false;
    
            return var1.first > var2.first;
        }
    };
    
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string,int> myMap;
        for (int i = 0; i < words.size(); ++i) ++myMap[words[i]];
        
        priority_queue<pair<string,int>,vector<pair<string,int>>,comparator> pq;
        unordered_map<string,int>::iterator iter = myMap.begin();
        while (iter != myMap.end()) {
            pq.push(*iter);
            ++iter;
        }
    
        vector<string> result;
        for (int i = 0; i < k; ++i) {
            result.push_back(pq.top().first);
            pq.pop();
        }
    
        return result;
    }
};
```