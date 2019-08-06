---
layout: post
title:  "453. Minimum Moves to Equal Array Elements"
date: 2019-08-05 19:01:00 -0400
categories: articles
---
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.

Example:
```
Input:
[1,2,3]

Output:
3
```
Explanation:
Only three moves are needed (remember each move increments two elements):

[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]

```c++
class Solution{
public:
    int minMoves(vector<int> &nums){
        if (nums.empty())
            return 0;
        int mi = nums[0];
        long sum = 0L;
        for (auto &num : nums){
            mi = min(mi, num);
            sum += num;
        }
        return sum - nums.size() * mi;
    }
};
```

```c++
class Solution {
public:
    int minMoves(vector<int>& ar) {
        int n = ar.size();
        int max1 = *min_element(ar.begin(),ar.end());
        long res = 0;
        for(int i = 0;i<n;i++)
        {
            res = res + (ar[i]-max1);
        }
        return res;
    }
};
```

```c++
class Solution {
public:
    int minMoves(vector<int>& nums) {
        int nmin=nums[0];
        for(int i=1;i<nums.size();i++){
            if(nums[i]<nmin) nmin=nums[i];
        }
        int s=0;
        for(int n: nums) s+=n-nmin;
        return s;
    }
};
```