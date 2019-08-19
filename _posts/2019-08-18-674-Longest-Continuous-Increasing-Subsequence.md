---
layout: post
title:  "674. Longest Continuous Increasing Subsequence"
date: 2019-08-18 16:55:00 -0400
categories: articles
---
Given an unsorted array of integers, find the length of longest continuous increasing subsequence (subarray).

Example 1:
```
Input: [1,3,5,4,7]
Output: 3
```
Explanation: 
```
The longest continuous increasing subsequence is [1,3,5], its length is 3. 
Even though [1,3,5,7] is also an increasing subsequence, it's not a continuous one where 5 and 7 are separated by 4. 
```
Example 2:
```
Input: [2,2,2,2,2]
Output: 1
```
Explanation: 
```
The longest continuous increasing subsequence is [2], its length is 1. 
```

```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size() == 0) return 0;
        int max = 1,count=1;
        for(int i =1;i < nums.size();i = i+1){
            if(nums[i] > nums[i-1]){
                count = count+1;
            }else{
                count = 1;
            }
            if(count > max){
                max = count;
            }
        }
        return max;
    }
};
```
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        int n = nums.size(), left = 0, ans = 0;
        while(left < n){
            int right = 1 + left;
            while(right < n && nums[right] > nums[right-1]){
                ++right;
            }
            ans = max(ans, right - left);
            left = right;
        }
        return ans;
    }
};
```
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.size()==0) return 0;
        int p,c,len;
        int max=1;
        p = nums[0];
        len=1;
        for(int i=1;i<nums.size();i++){
            c = nums[i];
            if(p<c){
                p = c;
                len++;
            }
            else{
                p=c;
                len = 1;  
            }
            if(max<len){
                max = len;
            }
        }
        return max;
    }
};
```
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(nums.empty())  return 0;
        
        int cur = 1, longest = 1;
        for(int i = 1; i < nums.size(); i ++) {
            if(nums[i] > nums[i-1]) {
                cur ++;
                longest = max(cur, longest);
            } else cur = 1;        
        }
        return longest;
    }
};
```
```C++
class Solution {
public:
    int findLengthOfLCIS(vector<int>& nums) {
        if(!nums.size() || nums.size() == 1) return nums.size();
        int mx_len = INT_MIN;
        int k = 1;
        for(int i = 1; i < nums.size(); i++) {
            if(nums[i] > nums[i-1]) k++;
            else {
                mx_len = max(mx_len, k);
                k = 1;
            }
        }
        return max(mx_len, k);
    }
};
```