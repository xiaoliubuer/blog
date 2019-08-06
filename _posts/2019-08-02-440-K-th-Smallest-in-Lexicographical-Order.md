---
layout: post
title:  "440. K-th Smallest in Lexicographical Order"
date: 2019-08-02 20:15:00 -0400
categories: articles
---
Given integers n and k, find the lexicographically k-th smallest integer in the range from 1 to n.

Note: 1 ≤ k ≤ n ≤ 109.

Example:
```
Input:
n: 13   k: 2

Output:
10
```
Explanation:
```
The lexicographical order is [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9], so the second smallest number is 10.
```
```c++
class Solution {
public:
    int findKthNumber(int n, int k) {
        stack<int> stk;
        stk.push(1);
        
        while(k){
            if(k == 1) return stk.top();
            long long top = stk.top(), x = top;
            stk.pop();
            long long sum = 0, mask = 1;
            
            while(top <= n){
                if(top + mask - 1 <= n) sum += mask;
                else sum += n - top + 1;
                top *= 10;
                mask *= 10;
            }
            
            if(k > sum){
                k -= sum;
                stk.push(x+1);
            }else{
                k --;
                stk.push(x*10);
            }
        }
        return stk.top();
    }
};
```