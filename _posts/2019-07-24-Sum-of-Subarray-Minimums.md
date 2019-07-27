---
layout: post
title:  "907. Sum of Subarray Minimums"
date: 2019-07-24 19:32:00 -0400
categories: articles
---
Given an array of integers A, find the sum of min(B), where B ranges over every (contiguous) subarray of A.

Since the answer may be large, return the answer modulo 10^9 + 7.
Example 1:
```
Input: [3,1,2,4]
Output: 17
Explanation: Subarrays are [3], [1], [2], [4], [3,1], [1,2], [2,4], [3,1,2], [1,2,4], [3,1,2,4]. 
Minimums are 3, 1, 2, 4, 1, 1, 2, 1, 1, 1.  Sum is 17.
```

```c++
class Solution {
public:
     int sumSubarrayMins(vector<int>& A) {
        stack<int> s;
        int size = A.size(), sum = 0, mod = 1e9 +7;
        A.push_back(0);
        for(int i = 0; i <= size; i++){
            while(!s.empty() && A[i] < A[s.top()]){
                int j = s.top();
                s.pop();
                int left = j - (s.empty() ? -1 : s.top());
                sum = (sum + left * (i - j) * A[j]) % mod;
            }
            s.push(i);
        }
        return sum;
    }
};
```
```c++
class Solution {
public:
    struct State{
        long long val;
        long long len;
        long long accumulate;
    };    
    
    int sumSubarrayMins(vector<int>& A) {
        const long long mod = 1e9 + 7;
        long long sum = 0;
        vector<State> v;
        for(auto x : A){
            State s{x, 1, 0};
            while(!v.empty() && v.back().val > x){
                s.len += v.back().len;
                v.pop_back();
            }
            s.accumulate = s.val * s.len;
            if(!v.empty()) s.accumulate += v.back().accumulate;
            s.accumulate %= mod;
            v.push_back(s);
            sum += s.accumulate;
        }
        return sum % mod;
    }
};
```