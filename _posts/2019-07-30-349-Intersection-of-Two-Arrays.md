---
layout: post
title:  "349. Intersection of Two Arrays"
date: 2019-07-30 20:36:00 -0400
categories: articles
---
Given two arrays, write a function to compute their intersection.

Example 1:
```
Input: nums1 = [1,2,2,1], nums2 = [2,2]
Output: [2]
```
Example 2:
```
Input: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
Output: [9,4]
```
Note:
```
Each element in the result must be unique.
The result can be in any order.
```
```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int>ans;
        unordered_set<int>hash;
        for (int i = 0; i < nums1.size(); ++i) hash.insert(nums1[i]);
        for (int i = 0; i < nums2.size(); ++i) {
            if (hash.count(nums2[i])) {
                ans.push_back(nums2[i]);
                hash.erase(nums2[i]);
            }
        }
        return ans;
    }
};
```

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.empty() || nums2.empty()){
            return std::vector<int>();
        }
        std::unordered_set<int> set{nums1.cbegin(), nums1.cend()};
        std::vector<int> intersections;
        for (auto n: nums2){
            if (set.erase(n) > 0){ // if n exists in set, then 1 is returned and n is erased; otherwise, 0.
                intersections.push_back(n);
            } 
        }
        return intersections;
    }
};
```
```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set1;
        unordered_set<int> set2;
        vector<int> res;
        for (auto& num1 : nums1) {
            set1.insert(num1);
        }
        for (auto& num2 : nums2) {
            if (set1.count(num2) > 0)
                set2.insert(num2);
        }
        for (auto& num : set2) {
            res.push_back(num);
        }
        return res;
    }
};
```
```c++

class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) 
    {
        vector<int>sol;
        unordered_map<int, int> un;
        for(int i = 0; i < nums1.size(); i++){
            if(un.find(nums1[i]) == un.end())
                un[nums1[i]] = 1;
        }
        for(int i = 0; i < nums2.size(); i++){
            if(un.find(nums2[i]) != un.end()){
                un[nums2[i]]++;
            }
        }
        auto it = un.begin();
        while(it != un.end()) {
            if(it->second > 1)
                sol.push_back(it->first);
            it++;
        }
        return sol;
    }
};
```