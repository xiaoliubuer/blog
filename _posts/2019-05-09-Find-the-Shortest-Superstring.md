---
layout: post
title:  "943. Find the Shortest Superstring"
date: 2019-05-09 17:38:00 -0400
categories: articles
---
Given an array A of strings, find any smallest string that contains each string in A as a substring.

We may assume that no string in A is substring of another string in A.

 
Example 1:
```
Input: ["alex","loves","leetcode"]
Output: "alexlovesleetcode"
Explanation: All permutations of "alex","loves","leetcode" would also be accepted.
```
Example 2:
```
Input: ["catg","ctaagt","gcta","ttca","atgcatc"]
Output: "gctaagttcatgcatc"
```
Note:

1 <= A.length <= 12
1 <= A[i].length <= 20
 

```c++
class Solution {
public:
    string shortestSuperstring(vector<string>& A) {
        int n = A.size();
        // dp[mask][i] : min superstring made by strings in mask,
        // and the last one is A[i]
        vector<vector<string>> dp(1<<n,vector<string>(n));
        vector<vector<int>> overlap(n,vector<int>(n,0));
        
        // 1. calculate overlap for A[i]+A[j].substr(?)
        for(int i=0; i<n; ++i) for(int j=0; j<n; ++j) if(i!=j){
            for(int k = min(A[i].size(), A[j].size()); k>0; --k)
                if(A[i].substr(A[i].size()-k)==A[j].substr(0,k)){
                    overlap[i][j] = k; 
                    break;
                }
        }
        // 2. set inital val for dp
        for(int i=0; i<n; ++i) dp[1<<i][i] += A[i];
        // 3. fill the dp
        for(int mask=1; mask<(1<<n); ++mask){
            for(int j=0; j<n; ++j) if((mask&(1<<j))>0){
                for(int i=0; i<n; ++i) if(i!=j && (mask&(1<<i))>0){
                    string tmp = dp[mask^(1<<j)][i]+A[j].substr(overlap[i][j]);
                    if(dp[mask][j].empty() || tmp.size()<dp[mask][j].size())
                        dp[mask][j] = tmp;
                }
            }
        }
        // 4. get the result
        int last = (1<<n)-1;
        string res = dp[last][0];
        for(int i=1; i<n; ++i) if(dp[last][i].size()<res.size()){
            res = dp[last][i];
        }
        return res;
    }
};
```