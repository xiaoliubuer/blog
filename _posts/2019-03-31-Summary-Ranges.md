---
layout: post
title:  "228. Summary Ranges"
date: 2019-03-31 11:36:23 -0400
categories: articles
---
Given a sorted integer array without duplicates, return the summary of its ranges.

Example 1:
```
Input:  [0,1,2,4,5,7]
Output: ["0->2","4->5","7"]
Explanation: 0,1,2 form a continuous range; 4,5 form a continuous range.
```
Example 2:
```
Input:  [0,2,3,4,6,8,9]
Output: ["0","2->4","6","8->9"]
Explanation: 2,3,4 form a continuous range; 8,9 form a continuous range.
```

```c++
class Solution {
public:
    vector<string> summaryRanges(vector<int>& nums){
    vector<string> ret;
    int n=nums.size();
    if(0==n)
        return ret;
    int p1=0;
    int p2=1;
    while(p1<n){
        while(p2<n && nums[p2-1]==nums[p2]-1) ++p2;

        if(p1 < p2-1){
            ret.push_back((to_string(nums[p1])+"->"+to_string(nums[p2-1])));
        }else{
            ret.push_back(to_string(nums[p1]));
        }
        p1=p2;
        ++p2;
    }
    return ret;
}
};
```