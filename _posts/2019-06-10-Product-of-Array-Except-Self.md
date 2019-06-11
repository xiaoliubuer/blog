---
layout: post
title:  "238. Product of Array Except Self"
date: 2019-06-10 22:12:00 -0400
categories: articles
---
Given an array nums of n integers where n > 1,  return an array output such that output[i] is equal to the product of all the elements of nums except nums[i].

Example:
```
Input:  [1,2,3,4]
Output: [24,12,8,6]
Note: Please solve it without division and in O(n).
```
Follow up:
```
Could you solve it with constant space complexity? (The output array does not count as extra space for the purpose of space complexity analysis.)
```

```c++
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = nums.size();
        vector<int> fromBegin(n);
        fromBegin[0] = 1;
        vector<int> fromLast(n);
        fromLast[0] = 1;
        
        for( int i = 1; i < n; i++ ){
            fromBegin[i] = fromBegin[i-1] * nums[i-1];
            fromLast[i]  = fromLast[i-1] * nums[n-i]; // Important to understand!!
        }

        // fromBegin: [1, 1, 2,  6]
        // fromLast:  [1, 4, 12, 24]
        
        vector<int> res(n);
        for( int i = 0; i < n; i++ ){
            res[i] = fromBegin[i] * fromLast[n-1-i]; // Important to understand!!
        }
        return res;
    }
};

```