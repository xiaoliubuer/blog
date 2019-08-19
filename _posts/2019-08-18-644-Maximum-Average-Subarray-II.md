---
layout: post
title:  "644. Maximum Average Subarray II"
date: 2019-08-18 15:26:00 -0400
categories: articles
---
Given an array consisting of n integers, find the contiguous subarray whose length is greater than or equal to k that has the maximum average value. And you need to output the maximum average value.

Example 1:
```
Input: [1,12,-5,-6,50,3], k = 4
Output: 12.75
```
Explanation:
```
when length is 5, maximum average value is 10.8,
when length is 6, maximum average value is 9.16667.
Thus return 12.75.
```
Note:
```
1 <= k <= n <= 10,000.
Elements of the given array will be in range [-10,000, 10,000].
The answer with the calculation error less than 10-5 will be accepted.
```
```c++
class Solution {
public:
    bool is_valid(vector<int>& nums, int k, double target) {
        vector<double> data(nums.size() + 1, 0.0);
        
        double sum = 0.0;
        for (int i = 1; i <= nums.size(); i++) {
            data[i] = data[i - 1] + nums[i - 1] - target;
            if (i >= k) sum = min(sum, data[i - k]);
            if (i >= k && data[i] > sum) return true; 
        }
        
        return false;
    }
    
    double findMaxAverage(vector<int>& nums, int k) {
        double start = INT32_MIN, end = accumulate(nums.begin(), nums.end(), 0.0);
        
        while (start + 0.0000001 < end) {
            double mid = (start + end) / 2;
            
            if (is_valid(nums, k, mid)) {
                start = mid;
            } else {
                end = mid;
            }
        }
        
        return start;
    }
};
```
```c++
class Solution {
public:
    double findMaxAverage(vector<int>& nums, int k) {
        vector<double> prefixSum(nums.size() + 1, 0);
        double start = 0, end = nums[0];
        for (int i = 0; i < nums.size(); i++) {
            prefixSum[i+1] = prefixSum[i] + nums[i];
            end = max(end, (double)nums[i]);
        }
        start = prefixSum.back() / nums.size();
        while ((end - start) > 5e-6) {
            double mid = start + (end - start) / 2;
            // ps[i] - ps[j] >= mid * (i - j)
            // => ps[i] - mid * i >= ps[j] - mid * j
            bool valid = false;
            double minPrefix = 0;
            for (int i = k; i < prefixSum.size(); i++) {
                double target = prefixSum[i] - mid * i;
                if (target >= minPrefix) {
                    valid = true;
                    break;
                } else {
                    minPrefix = min(minPrefix, prefixSum[i - k + 1] - mid * (i-k+1));
                }
            }
            if (valid) {
                start = mid;
            } else {
                end = mid;
            }
        }
        return end;
    }
};
```
```c++
class Solution {
public:
double findMaxAverage(vector<int>& nums, int k) {
    int n = nums.size();
    double sum[n+1] = {0};
    double l = *min_element(nums.begin(), nums.end()), r=*max_element(nums.begin(), nums.end());        
    while(r-l > 1e-5){
        double min_sum=0, mid=(l+r)/2;
        int i=1, check=0;
        for(;i<k; i++) sum[i] = sum[i-1]+ nums[i-1]-mid;      
        for(;i<=n;i++) {
            sum[i] = sum[i-1]+ nums[i-1]-mid;
            if(sum[i]- min_sum>0) { check=1; break;  }  // without negative part, sum[i]- min_sum is the maximal sum
            if(sum[i-k+1]<min_sum) min_sum = sum[i-k+1];   // update min_sum, which is always negative 
        }
        if(check) l=mid;
        else r=mid;
    }
    return l; 
}
};
```