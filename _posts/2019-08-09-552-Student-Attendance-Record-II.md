---
layout: post
title:  "552. Student Attendance Record II"
date: 2019-08-09 21:28:00 -0400
categories: articles
---
Given a positive integer n, return the number of all possible attendance records with length n, which will be regarded as rewardable. The answer may be very large, return it after mod 109 + 7.

A student attendance record is a string that only contains the following three characters:
```
'A' : Absent.
'L' : Late.
'P' : Present.
```
A record is regarded as rewardable if it doesn't contain more than one 'A' (absent) or more than two continuous 'L' (late).

Example 1:
```
Input: n = 2
Output: 8 
```
Explanation:
```
There are 8 records with length 2 will be regarded as rewardable:
"PP" , "AP", "PA", "LP", "PL", "AL", "LA", "LL"
Only "AA" won't be regarded as rewardable owing to more than one absent times. 
```
Note: The value of n won't exceed 100,000.
```c++
class Solution {
public:
    int checkRecord(int n) {
        // 0 - no absent
        // 1 - no absent and ending with late
        // 2 - one absent
        // 3 - one absent and ending with late
        vector<vector<long long int> > dp(n+1, vector<long long int>(4)); 
        
        long long int p = 10e8 + 7;
        
        dp[1][0] = 1; // 'P'
        dp[1][1] = 1; // 'L'
        dp[1][2] = 1; // 'A'
        dp[1][3] = 0; // Not possible
        
        dp[0][0] = 1; // seeded so that DP works 
        
        for(int i = 2; i<=n; i++) {
            dp[i][0] = (dp[i-1][0] + dp[i-1][1])%p; // (present yesterday or late yesterday) and present today 
            dp[i][1] = (dp[i-1][0] + dp[i-2][0])%p; // (present yesterday and late today) or (present 2 days back and late for 2 days)
            dp[i][2] = (dp[i-1][2] + dp[i-1][3] + dp[i-1][0] + dp[i-1][1])%p;
            // ((present yesterday with one absent or late yesterday with one absent) and present today) or ((present yesterday with no absent or late yesterday with no absent) and absent today ) 
            dp[i][3] = (dp[i-1][2] + dp[i-2][2])%p;
            // (present yesterday and late today with one absent) or (present 2 days back and late for 2 days with one absent)  
        }
        
        return (dp[n][0] + dp[n][1] + dp[n][2] + dp[n][3]) % p;
    }
};
```
```c++

class Solution {
public:
    int checkRecord(int n) {
        long M = 1000000007;
        vector<long>::size_type N = 0;
        if(n <= 5)
            N = 6;
        else
            N = n + 1;
        vector<long> f(N, 0);
        f[0] = 1;
        f[1] = 2;
        f[2] = 4;
        f[3] = 7;
        for(N = 4; N < f.size(); ++N){
            f[N] = ((2*f[N-1])%M + (M-f[N-4])%M)%M;
        }
        long sum = f[n];
        for(int i = 1; i <= n; ++i){
            sum += (f[i-1]*f[n-i])%M;
            sum %= M;
        }
        return static_cast<int>(sum);
    }
};
```