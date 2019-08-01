---
layout: post
title:  "368. Largest Divisible Subset"
date: 2019-07-31 20:20:00 -0400
categories: articles
---
Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies:

Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

Example 1:
```
Input: [1,2,3]
Output: [1,2] (of course, [1,3] will also be ok)
```
Example 2:
```
Input: [1,2,4,8]
Output: [1,2,4,8]
```
```c++

class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        vector<int> out;
        if(nums.size()==0) return out;
        if(nums.size()==1) return nums;
        sort(nums.begin(), nums.end());
        vector<int> visited(nums.size(),1);
        vector<int> prev(nums.size(),-1);
        int max, maxInd;
        max=1;
        maxInd=0;
        for(int i=0; i<nums.size()-1; i++){
            for(int j=i+1; j<nums.size();j++){
                if(nums[j]%nums[i]==0){
                    if(visited[j]<visited[i]+1){
                        visited[j] = visited[i]+1;
                        prev[j]=i;
                        maxInd = visited[j]>max?j:maxInd;
                        max = visited[j]>max?visited[j]:max;
                        
                    }
                }
            }
            
        }
        while(maxInd>=0){
            out.push_back(nums[maxInd]);
            maxInd = prev[maxInd];
        }
        reverse(out.begin(),out.end());
        return out;
    }
};
```
```c++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        if(nums.size() <= 1) return nums;
        vector<int> dp(nums.size(), 1);
        vector<int> p(nums.size(), -1);
        int max_idx=0, m=0;
        sort(nums.begin(), nums.end());
        for(int i=0; i<nums.size(); ++i) {
            for(int j=0;j<i;++j) {
                if(nums[i] % nums[j] ==0 && dp[i] < dp[j] + 1) {
                    dp[i] = dp[j] + 1;
                    p[i] = j;
                    if(dp[i] > m) {
                        m = dp[i];
                        max_idx = i;
                    }
                }
            }
        }
        vector<int> ret;
        while(max_idx != -1) {
            ret.push_back(nums[max_idx]);
            max_idx = p[max_idx];
        }
        return vector<int> (ret.rbegin(), ret.rend());
    }
};
```
```c++
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int i,j,len=nums.size(),m=0,mi;
        vector<int> T(len,0);
        vector<int> son(len,0);
        sort(nums.begin(),nums.end());
        for(i=0;i<len;i++){
            for(j=i;j>=0;j--){
                if(nums[i]%nums[j]==0&&T[j]+1>T[i]){
                    T[i]=T[j]+1;
                    son[i]=j;
                }
            }
            if(T[i]>m){
                m=T[i];
                mi=i;
            }
        }
        vector<int> re;
        for(i=0;i<m;i++){
            re.insert(re.begin(),nums[mi]);
            mi=son[mi];
        }
        return re;
    }
};
```