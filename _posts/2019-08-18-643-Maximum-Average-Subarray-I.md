---
layout: post
title:  "643. Maximum Average Subarray I"
date: 2019-08-18 15:22:00 -0400
categories: articles
---
Given an array consisting of n integers, find the contiguous subarray of given length k that has the maximum average value. And you need to output the maximum average value.

Example 1:
```
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
Explanation: Maximum average is (12-5-6+50)/4 = 51/4 = 12.75
```

Note:
```
1 <= k <= n <= 30,000.
Elements of the given array will be in the range [-10,000, 10,000].
```
```c++
class Solution {
public:
 double findMaxAverage(vector<int>& nums, int k) {
        double sum=0, res=INT_MIN;
        for(int i=0;i<nums.size();i++) {
            if(i<k) sum+=nums[i];
            else {
                res=max(sum, res);
                sum+=nums[i]-nums[i-k];
            }
        }
        res=max(sum, res);
        return res/k;
    }
};
```
```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int curSum = 0;
        for (int i = 0; i < k; i++) {
            curSum += nums[i];
        }
        double maxAvg = curSum * 1.0 / k;
        int n = nums.size();
        for (int i = 1; i < n - k + 1; i++) {
            curSum += nums[i+k-1] - nums[i-1];
            maxAvg = max(maxAvg, curSum * 1.0 / k);
        }
        return maxAvg;
    }
};
```
```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        int mx=INT_MIN,curr=0;
        for(int i=0;i<k&&i<nums.size();i++)
            curr+=nums[i];
        mx=curr;
        for(int i=k;i<nums.size();i++)
        {
            curr=curr+nums[i]-nums[i-k];
            mx=max(mx,curr);
        }
        double ans=(double)mx/k;
        return ans;
    }
};
```