---
layout: post
title:  "347. Top K Frequent Elements"
date: 2019-07-30 20:30:00 -0400
categories: articles
---
Given a non-empty array of integers, return the k most frequent elements.

Example 1:
```
Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]
```
Example 2:
```
Input: nums = [1], k = 1
Output: [1]
```
Note:
```
You may assume k is always valid, 1 ≤ k ≤ number of unique elements.
Your algorithm's time complexity must be better than O(n log n), where n is the array's size.
```
```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> m;
        int n = nums.size();
        for(int i=0;i<n;++i) {
            m[nums[i]]++;
        }
        priority_queue<pair<int, int> > pq;
        for(auto x:m) {
            pq.push({x.second, x.first});
        }
        vector<int> v;
        while(!pq.empty() && k--) {
            v.push_back(pq.top().second); pq.pop();
        }
        return v;
    }
};
```
```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int> >> pq;
        unordered_map<int, int> cnt;
        for (auto num : nums) cnt[num]++;

        for (auto kv : cnt) {
            pq.push({kv.second, kv.first});
            while (pq.size() > k) pq.pop();
        }
        vector<int> res;
        while (!pq.empty()) {
            res.push_back(pq.top().second);
            pq.pop();
        }
        return res;
    }
};
```