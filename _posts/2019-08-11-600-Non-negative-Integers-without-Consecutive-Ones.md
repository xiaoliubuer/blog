---
layout: post
title:  "600. Non-negative Integers without Consecutive Ones"
date: 2019-08-11 18:25:00 -0400
categories: articles
---	
Given a positive integer n, find the number of non-negative integers less than or equal to n, whose binary representations do NOT contain consecutive ones.

Example 1:
```
Input: 5
Output: 5
```
Explanation: 
```
Here are the non-negative integers <= 5 with their corresponding binary representations:
0 : 0
1 : 1
2 : 10
3 : 11
4 : 100
5 : 101
Among them, only integer 3 disobeys the rule (two consecutive ones) and the other 5 satisfy the rule. 
```
Note: 1 <= n <= 109

```c++
class Solution {
public:
    int findI
    ntegers(int num) {
        if (num <= 2) return num + 1;
        vector<int> dp(32, 0); // dp[k] means how many answers if n == 2^k - 1
        dp[0] = 1;
        dp[1] = 2;
        int base = 2; // base = 2^k
        int k = 1;
        while (base * 2 <= num + 1) {
            k++;
            dp[k] = dp[k - 1] + dp[k - 2];
            base <<= 1;
        }
        num -= base; // if num == -1, it's done
        num = min(num, base/2 - 1); // since we remove the first '1', then if the first digit is '1' now, it's invalid.
        return dp[k] + findIntegers(num);
    }
};
```
```c++
class Solution {
public:
    int findIntegers(int num) {
        string s;
        while(num > 0){
            s += (num & 1) ? '1' : '0';
            num >>= 1;
        }
        int n = s.size();
        int z[n] = {0} ,o[n] = {0};
        z[0] = o[0] = 1;
        for(int i = 1; i < n; ++i){
            z[i] = z[i-1] + o[i-1];
            o[i] = z[i-1];
        }
        int ans = o[n-1] + z[n-1];
        n -= 2;
        while(n >= 0) {
            if(s[n] == s[n+1] && s[n] == '1') break;
            if(s[n] == s[n+1] && s[n] == '0') ans -= o[n];
            --n;
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int findIntegers(int num) {
        int f[32];
        f[0] = 1;
        f[1] = 2;
        for (int i = 2; i < 32; ++i)
            f[i] = f[i-1]+f[i-2];
        int ans = 0, k = 30, pre_bit = 0;
        while (k >= 0) {
            if (num&(1<<k)) {
                ans += f[k];
                if (pre_bit) return ans;
                pre_bit = 1;
            }
            else
                pre_bit = 0;
            --k;
        }
        return ans+1;
    }
};
```