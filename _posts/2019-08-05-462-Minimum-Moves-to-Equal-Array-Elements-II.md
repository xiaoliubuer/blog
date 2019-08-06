---
layout: post
title:  "462. Minimum Moves to Equal Array Elements II"
date: 2019-08-05 19:35:00 -0400
categories: articles
---
Given a non-empty integer array, find the minimum number of moves required to make all array elements equal, where a move is incrementing a selected element by 1 or decrementing a selected element by 1.

You may assume the array's length is at most 10,000.

Example:
```
Input:
[1,2,3]

Output:
2
```
Explanation:
```
Only two moves are needed (remember each move increments or decrements one element):

[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
```

```c++
class Solution {
public:
    int minMoves2(vector<int>& nums) {
        int n = nums.size();
        if(n < 2) return 0;
        int64_t minMoves = UINT_MAX;
        sort(nums.begin(), nums.end());
        vector<int64_t> leftMoves(n, 0);
        for(int i = 1; i < n; ++i) {
            leftMoves[i] = i * (nums[i] - nums[i-1]) + leftMoves[i-1];
        }
        int64_t rightMoves = 0;
        for(int i = n-2; i >= 0; --i) {
            rightMoves = (n-1-i) * (nums[i+1] - nums[i]) + rightMoves;
            minMoves = min(rightMoves + leftMoves[i], minMoves);
        }
        return minMoves;
    }
};
```