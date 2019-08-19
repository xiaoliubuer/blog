---
layout: post
title:  "673. Number of Longest Increasing Subsequence"
date: 2019-08-18 16:51:00 -0400
categories: articles
---
Given an unsorted array of integers, find the number of longest increasing subsequence.
Example 1:
```
Input: [1,3,5,4,7]
Output: 2
Explanation: The two longest increasing subsequence are [1, 3, 4, 7] and [1, 3, 5, 7].
```
Example 2:
```
Input: [2,2,2,2,2]
Output: 5
Explanation: The length of longest continuous increasing subsequence is 1, and there are 5 subsequences' length is 1, so output 5.
```
Note: Length of the given array will be not exceed 2000 and the answer is guaranteed to be fit in 32-bit signed int.
```c++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int n = nums.size();
        int mx = 1;
        vector<int> len(n, 1), cnt(n, 1);
        for(int i = 0; i < n; ++i){
            int a = nums[i];
            for(int j = 0; j < i; ++j) {
                int b = nums[j];
                if(a <=b ) continue;
                if(len[i] < len[j]+ 1){
                    len[i] = len[j] + 1;
                    cnt[i] = cnt[j];
                    mx = max(mx, len[i]);
                }else if(len[i] == len[j] + 1){
                    cnt[i] += cnt[j];
                }
                
            }
        }
        int ans = 0;
        for(int i = 0; i < n; ++i){
            if(len[i] == mx) ans += cnt[i];
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;
        vector<int> vCount(nums.size(), 1);
        vector<int> vLen(nums.size(), 1);
        int maxLen = 1;
        int count = 1;
        
        for(int i = 1; i < nums.size(); ++i)
        {
            for(int j = 0; j <= i - 1; ++j)
            {
                if(nums[j] < nums[i])
                {
                    if(vLen[j] + 1 == vLen[i])
                    {
                        vCount[i] += vCount[j];
                    }
                    else if(vLen[j] + 1 > vLen[i])
                    {
                        vCount[i] = vCount[j];
                        vLen[i] = vLen[j] + 1;
                    }
                }
            }
            
            if(maxLen < vLen[i])
            {
                maxLen = vLen[i];
                count = vCount[i];
            }
            else if(maxLen == vLen[i])
            {
                count += vCount[i];
            }
        }

        return count;
    }
};
```
```c++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        int size = nums.size();
        if (size<2) return size;
        
        int result = 0;
        int longest = 0;
        vector<int> dp(size,1);
        vector<int> dpfreq(size,1);
        
        for (int i=1 ; i<size ; i++) {
            int freq = 1;
            for (int j=0 ; j<i ; j++) {
                if (nums[i]>nums[j]) {
                    if (dp[i]==dp[j]+1) {
                        dpfreq[i] += dpfreq[j];
                    }
                    else if (dp[i]<dp[j]+1) {
                        dp[i] = dp[j]+1;
                        dpfreq[i] = dpfreq[j];
                    }
                }
            }
            if (longest==dp[i]) {
                result+=dpfreq[i];
            }
            else if (longest<dp[i]) {
                longest = dp[i];
                result = dpfreq[i];
            }
        }
        if (longest==1) 
            return size;
        
        return result;
    }
};
```
```c++
class Solution {
public:
    int findNumberOfLIS(vector<int>& nums) {
        if(nums.size() == 0)
            return 0;
        vector<int> vCount(nums.size(), 1);
        vector<int> vLen(nums.size(), 1);
        int maxLen = 1;
        int count = 1;
        
        for(int i = 1; i < nums.size(); ++i)
        {
            for(int j = 0; j <= i - 1; ++j)
            {
                if(nums[j] < nums[i])
                {
                    if(vLen[j] + 1 == vLen[i])
                    {
                        vCount[i] += vCount[j];
                    }
                    else if(vLen[j] + 1 > vLen[i])
                    {
                        vCount[i] = vCount[j];
                        vLen[i] = vLen[j] + 1;
                    }
                }
            }
            if(maxLen < vLen[i])
            {
                maxLen = vLen[i];
                count = vCount[i];
            }
            else if(maxLen == vLen[i])
            {
                count += vCount[i];
            }
        }
        return count;
    }
};
```