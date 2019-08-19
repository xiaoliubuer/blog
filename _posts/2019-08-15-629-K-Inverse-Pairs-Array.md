---
layout: post
title:  "629. K Inverse Pairs Array"
date: 2019-08-15 08:45:00 -0400
categories: articles
---	
Given two integers n and k, find how many different arrays consist of numbers from 1 to n such that there are exactly k inverse pairs.

We define an inverse pair as following: For ith and jth element in the array, if i < j and a[i] > a[j] then it's an inverse pair; Otherwise, it's not.

Since the answer may be very large, the answer should be modulo 109 + 7.

Example 1:
```
Input: n = 3, k = 0
Output: 1
```
Explanation: 
```
Only the array [1,2,3] which consists of numbers from 1 to 3 has exactly 0 inverse pair.
```
Example 2:
```
Input: n = 3, k = 1
Output: 2
```
Explanation: 
```
The array [1,3,2] and [2,1,3] have exactly 1 inverse pair.
```
Note:
```
The integer n is in the range [1, 1000] and k is in the range [0, 1000].
```
```c++
class Solution {
public:
    int kInversePairs(int n, int k) {
        long long mod = 1000000007;
        if(n==1&&k==1)return 0;
        vector<long long> arr(k+2,0);
        arr[0]=1;
        arr[1]=1;
        int num=1;
        for(int i = 2; i <= n-1; i++ ) {
            
            num*=i+1;
            num/=i-1;
            if(num>k)num=k;
            for (int j = 1; j<= num; j++) {
                arr[j]+=arr[j-1];
            }
            for (int j = num; j >= 0; j--) {
                if (j-i-1 >= 0)
                    arr[j] -= arr[j-i-1];
                
                arr[j]%=mod;
            }
        }
        
        return (int)arr[k];
    }
};
```
```c++
class Solution {
public:
    const int Mo = 1000000007;
    int f[2000][2000];
    int kInversePairs(int n, int k) {
        f[1][0] = 1;
        for(int i = 2; i <= n; i++) {
            long long tmp = 0;
            for(int j = 0; j <= min(k, i * (i - 1) / 2); j++) {
                tmp = (tmp + f[i - 1][j]) % Mo;
                if  (j >= i) tmp = (tmp - f[i - 1][j - i]) % Mo;
                f[i][j] = tmp;
            }
        }
        return (f[n][k] % Mo + Mo) % Mo;
    }
};
```

```c++
class Solution {
    long long base = 1000000007;
public:
    int kInversePairs(int n, int k) {
        vector<int> dp0 (k + 1), dp1(k + 1);
        dp0[0] = dp1[0] = 1;
        for (int i = 1; i <= n; ++ i)
        {
            for (int j = 1; j <= k; ++ j)
                dp1[j] = (dp1[j - 1] + dp0[j] + base - (j - i >= 0 ? dp0[j - i] : 0)) % base; 
            swap(dp0, dp1); //swap costs O(1) in C++
        }
        return dp0.back();
    }
};
```
```c++
class Solution {
private: 
    int MOD = 1000000007;
public:
    int kInversePairs(int n, int k) {
        int dp[n + 1][k + 1];
        memset(dp, 0, sizeof dp);

        for(int i = 1; i <= n; i++) {
            for(int j = 0; j <= k && j <= ((i - 1)*i)/2; j++) {
                if(j == 0) dp[i][j] = 1;
                else {
                    int val = (dp[i - 1][j] + MOD - ((j >= i) ? dp[i - 1][j - i] : 0))%MOD;
                    dp[i][j] = (dp[i][j - 1] + val) % MOD;
                }
            }
        }

        return dp[n][k];
    }
};
```