---
layout: post
title:  "632. Smallest Range Covering Elements from K Lists"
date: 2019-08-18 14:52:00 -0400
categories: articles
---	
You have k lists of sorted integers in ascending order. Find the smallest range that includes at least one number from each of the k lists.

We define the range [a,b] is smaller than range [c,d] if b-a < d-c or a < c if b-a == d-c.

Example 1:
```
Input: [[4,10,15,24,26], [0,9,12,20], [5,18,22,30]]
Output: [20,24]
```
Explanation: 
```
List 1: [4, 10, 15, 24,26], 24 is in range [20,24].
List 2: [0, 9, 12, 20], 20 is in range [20,24].
List 3: [5, 18, 22, 30], 22 is in range [20,24].
```
Note:
```
The given list may contain duplicates, so ascending order means >= here.
1 <= k <= 3500
-105 <= value of elements <= 105.
```
```c++
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        vector<int> res;
        vector<pair<int, int>> v;
        unordered_map<int, int> m;
        for (int i = 0; i < nums.size(); ++i) {
            for (int num : nums[i]) {
                v.push_back({num, i});
            }
        }
        sort(v.begin(), v.end());
        int left = 0, n = v.size(), k = nums.size(), cnt = 0, diff = INT_MAX;
        for (int right = 0; right < n; ++right) {
            if (m[v[right].second] == 0) ++cnt;
            ++m[v[right].second];
            while (cnt == k && left <= right) {
                if (diff > v[right].first - v[left].first) {
                    diff = v[right].first - v[left].first;
                    res = {v[left].first, v[right].first};
                } 
                if (--m[v[left].second] == 0) --cnt;
                ++left;
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
        typedef vector<int>::iterator vi;
        
        struct comp {
            bool operator()(pair<vi, vi> p1, pair<vi, vi> p2) {
                return *p1.first > *p2.first;
            }
        };
        
        int lo = INT_MAX, hi = INT_MIN;
        priority_queue<pair<vi, vi>, vector<pair<vi, vi>>, comp> pq;
        for (auto &row : nums) {
            lo = min(lo, row[0]);
            hi = max(hi, row[0]);
            pq.push({row.begin(), row.end()});
        }
        
        vector<int> ans = {lo, hi};
        while (true) {
            auto p = pq.top();
            pq.pop();
            ++p.first;
            if (p.first == p.second)
                break;
            pq.push(p);
            
            lo = *pq.top().first;
            hi = max(hi, *p.first);
            if (hi - lo < ans[1] - ans[0])
                ans = {lo, hi};
        }
        return ans;
    }
};
```
```c++
class Element{
public:
    int row;
    int column;
    int value;
    Element(int r,int c,int v){
        row = r;
        column = c;
        value = v;
    }
};
class compare{
public:
   bool operator()(Element &a, Element &b){
       return a.value > b.value;
   }
    
};
class Solution {
public:
    vector<int> smallestRange(vector<vector<int>>& nums) {
       vector<int>result(2,0); 
       // smallest range
       // because every time when you pop an element, it will replace with the same list
       priority_queue<Element,vector<Element>,compare>minheap;
        int maxV = INT_MIN;
        int range = INT_MAX;
       for(int i = 0; i < nums.size(); i++){
           minheap.push(Element(i,0,nums[i][0]));
           maxV = max(maxV,nums[i][0]);
           
       }
      
        while(minheap.size() == nums.size()){
            Element current = minheap.top();
            minheap.pop();
            if(range > maxV - current.value){
                range = maxV - current.value;
                result[0] = current.value;
                result[1] = maxV;
            } 
            if(current.column+1 < nums[current.row].size()){
                minheap.push(Element(current.row,current.column+1,nums[current.row][current.column+1]));
                maxV = max(maxV,nums[current.row][current.column+1]);
            }
            
        }
        return result;
        
    }
};
```