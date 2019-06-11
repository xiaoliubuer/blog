---
layout: post
title:  "152. Maximum Product Subarray"
date: 2019-06-10 21:34:00 -0400
categories: articles
---
Given an integer array nums, find the contiguous subarray within an array (containing at least one number) which has the largest product.

Example 1:
```
Input: [2,3,-2,4]
Output: 6
Explanation: [2,3] has the largest product 6.
```
Example 2:
```
Input: [-2,0,-1]
Output: 0
Explanation: The result cannot be 2, because [-2,-1] is not a subarray.
```

# Best Solution
O(N)

Maintain imax and imin to keep the largest and smallest.

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int imax = nums[0];
        int imin = nums[0];
        int res  = nums[0];
        for( int i = 1; i < nums.size(); i++ ){
            if( nums[i] < 0 ) swap( imax, imin );
            imax = max( imax * nums[i], nums[i]);
            imin = min( imin * nums[i], nums[i]);
            res =  max( imax, res );
        }
        return res;
    }
};
```

