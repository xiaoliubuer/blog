

---
layout: post
title:  "698. Partition to K Equal Sum Subsets"
date: 2019-08-18 18:23:00 -0400
categories: articles
---
Given an array of integers nums and a positive integer k, find whether it's possible to divide this array into k non-empty subsets whose sums are all equal.

Example 1:
```
Input: nums = [4, 3, 2, 3, 5, 2, 1], k = 4
Output: True
Explanation: It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.
```
Note:
```
1 <= k <= len(nums) <= 16.
0 < nums[i] < 10000.
```

```c++
class Solution {
 public:
  bool canPartitionKSubsets(vector<int> &nums, int k) {
    int sum = 0;
    for (int i = 0; i < nums.size(); i++) {
      sum += nums[i];
    }
    if (sum % k != 0) return false;
    int target = sum / k;
    sort(nums.begin(), nums.end(), greater<int>());
    vector<bool> isVisited(nums.size(), false);
    return partitionKSubsets(nums, k, 0, 0, isVisited, target);
  }
  bool partitionKSubsets(vector<int> &nums, int k, int start, int curSum, vector<bool> &isVisited, int target) {
    if (k == 1) return true;
    if (curSum > target) return false;
    if (curSum == target) return partitionKSubsets(nums, k - 1, 0, 0, isVisited, target);
    for (int i = start; i < nums.size(); i++) {
      if (isVisited[i]) continue;
      isVisited[i] = true;
      if (partitionKSubsets(nums, k, i + 1, curSum + nums[i], isVisited, target)) return true;
      isVisited[i] = false;
    }
    return false;
  }
};
```

```c++
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& arr, int k) 
    {
        int sum=0,n=arr.size();
        for(int i=0;i<arr.size();i++)
        {
            sum+=arr[i];
        }
        if(sum%k!=0)
            return 0;
        int r=sum/k;
        bool vis[n+2]={0};
        return part(0,arr,vis,0,sum/k,k);   
    }
    bool part(int start,vector<int>&arr,bool vis[],int curr,int target,int k)
    {
        if(k==1)
            return 1;
        if(curr==target)
        {
            return part(0,arr,vis,0,target,k-1);   
        }
        for(int i=start;i<arr.size();i++)
        {
            if(vis[i]==false)
            {
                vis[i]=true;
                int d=part(i+1,arr,vis,curr+arr[i],target,k);
                if(d==1)
                    return true;
                else
                {
                    vis[i]=false;
                }
            }
        }
        return false;
    }
};
```
```c++
class Solution {
public:
    bool canPartitionKSubsets(vector<int>& nums, int k) {
        int total = 0;
        int n = nums.size();
        sort(nums.begin(), nums.end(), greater<int>());
        for (int i = 0; i < n; i++) {
            total += nums[i];
        }
        if (total % k != 0) {
            return false;
        }
        total /= k;
        vector<int> curSum(k, 0);
        return helper(nums, 0, curSum, total);
    }

    bool helper(const vector<int>& nums, int index, vector<int>& curSum, int total) {
        if (index == nums.size()) {
            for (int i = 0; i < curSum.size(); i++) {
                if (curSum[i] != total) return false;
            }
            return true;
        }
        unordered_set<int> visited;
        for (int i = 0; i < curSum.size(); i++) {
            if (curSum[i] + nums[index] <= total && visited.count(curSum[i]) == 0) {
                visited.insert(curSum[i]);
                curSum[i] += nums[index];
                bool ans = helper(nums, index + 1, curSum, total);
                if (ans) return true;
                curSum[i] -= nums[index];
            }
        }
        return false;
    }
};
```