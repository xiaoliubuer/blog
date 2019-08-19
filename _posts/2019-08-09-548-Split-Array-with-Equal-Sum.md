---
layout: post
title:  "548. Split Array with Equal Sum"
date: 2019-08-09 21:15:00 -0400
categories: articles
---
Given an array with n integers, you need to find if there are triplets (i, j, k) which satisfies following conditions:
```
0 < i, i + 1 < j, j + 1 < k < n - 1
Sum of subarrays (0, i - 1), (i + 1, j - 1), (j + 1, k - 1) and (k + 1, n - 1) should be equal.
```
where we define that subarray (L, R) represents a slice of the original array starting from the element indexed L to the element indexed R.
Example:
```
Input: [1,2,1,2,1,2,1]
Output: True
Explanation:
i = 1, j = 3, k = 5. 
sum(0, i - 1) = sum(0, 0) = 1
sum(i + 1, j - 1) = sum(2, 2) = 1
sum(j + 1, k - 1) = sum(4, 4) = 1
sum(k + 1, n - 1) = sum(6, 6) = 1
```
Note:
```
1 <= n <= 2000.
Elements in the given array will be in range [-1,000,000, 1,000,000].
```
```c++
class Solution {
public:
    bool splitArray(vector<int>& nums) {
        unordered_map<long, vector<int>> jump;
        jump[nums[0]].push_back(0);
        
        for (int i=1; i < nums.size(); ++i) {
            nums[i] += nums[i-1]; 
            jump[nums[i]].push_back(i);
        }
        
        for (int endIdx = nums.size() - 2; endIdx >= 4; --endIdx) {
            long endSum = nums.back() - nums[endIdx];
    
            for (int startIdx: jump[endSum]) {
                for (int j = startIdx + 1; j+1 < endIdx; ++j) {
                    long secondSum = nums[j] - nums[startIdx+1];
                    long thridSum = nums[endIdx-1] - nums[j+1];
                        
                    if (secondSum == thridSum && secondSum == endSum) return true;
                }
            }
        }
        
        return false;
    }
};
```