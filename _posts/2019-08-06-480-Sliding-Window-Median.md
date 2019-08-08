---
layout: post
title:  "480. Sliding Window Median"
date: 2019-08-06 19:56:00 -0400
categories: articles
---
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

Examples: 
```
[2,3,4] , the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5
```
Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Your job is to output the median array for each window in the original array.

For example,
```
Given nums = [1,3,-1,-3,5,3,6,7], and k = 3.

Window position                Median
---------------               -----
[1  3  -1] -3  5  3  6  7       1
 1 [3  -1  -3] 5  3  6  7       -1
 1  3 [-1  -3  5] 3  6  7       -1
 1  3  -1 [-3  5  3] 6  7       3
 1  3  -1  -3 [5  3  6] 7       5
 1  3  -1  -3  5 [3  6  7]      6
Therefore, return the median sliding window as [1,-1,-1,3,5,6].
```
Note: 
You may assume k is always valid, ie: k is always smaller than input array's size for non-empty array.

```c++
class Solution {
public:
multiset <double> m;
    
    double findMedian(double remove, double add) {
        m.insert(add);
        m.erase(m.find(remove));
        int n = m.size();
        double a = *next(m.begin(), n/2 - 1);
        double b = *next(m.begin(), n/2);
        return n & 1? b : (a + b) * 0.5;
    }
    
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
        vector <double> ans;
        if(nums.size() < k)
            return ans;
        for(int i=0; i<k; i++) {
            m.insert(nums[i]);
        }
        ans.push_back(findMedian(0, 0));
        for(int i=k; i<nums.size(); i++) {
            ans.push_back(findMedian(nums[i-k], nums[i]));
        }
        return ans;
    }
};
```
```c++
class Solution {
    vector<double> result;
    multiset<int> s1, s2;

public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k)
    {
        for (int i = 0; i < k; i++)
            insert(nums[i]);
        median();
        for (int i = k; i < nums.size(); i++)
            insert(nums[i]), remove(nums[i - k]), median();
        return result;
    }
    void remove(int n)
    {
        auto it = s1.find(n);
        if (it != s1.end())
            s1.erase(it);
        else
            s2.erase(s2.find(n));
        balance();
    }
    void median()
    {
        if (s1.size() > s2.size())
            result.push_back(*s1.begin());
        else if (s1.size() < s2.size())
            result.push_back(*s2.rbegin());
        else
            result.push_back((0.0 + *s1.begin() + *s2.rbegin()) / 2);
    }
    void insert(int num)
    {
        if (s1.empty() && s2.empty())
            s1.insert(num);
        else if (s2.empty())
            s1.insert(num);
        else if (s1.empty())
            s2.insert(num);
        else if (num >= *s1.begin())
            s1.insert(num);
        else
            s2.insert(num);
        balance();
    }
    void balance()
    {
        if (s1.size() > 1 + s2.size())
            s2.insert(*s1.begin()), s1.erase(s1.find(*s1.begin()));
        else if (s1.size() + 1 < s2.size())
            s1.insert(*s2.rbegin()), s2.erase(s2.find(*s2.rbegin()));
    }
};
```
```c++
class Solution {
public:
    vector<double> medianSlidingWindow(vector<int>& nums, int k) {
         vector<double> medians;
        multiset<int> window(nums.begin(), nums.begin() + k);
        
        auto mid = next(window.begin(), k / 2);
        
        for (int i = k;;i++) {
            medians.push_back(((double)(*mid) + *next(mid, k % 2 - 1)) / 2.0);
            
            if (i == nums.size()) break;
            
            window.insert(nums[i]);
            if (nums[i] < *mid) 
                mid--;
            
            if (nums[i-k] <= *mid)
                mid++;
            
            window.erase(window.lower_bound(nums[i-k]));
        }
        
        return medians;
    }
};
```