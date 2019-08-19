---
layout: post
title:  "668. Kth Smallest Number in Multiplication Table"
date: 2019-08-18 16:37:00 -0400
categories: articles
---
Nearly every one have used the Multiplication Table. But could you find out the k-th smallest number quickly from the multiplication table?

Given the height m and the length n of a m * n Multiplication Table, and a positive integer k, you need to return the k-th smallest number in this table.

Example 1:
```
Input: m = 3, n = 3, k = 5
Output: 
```
Explanation: 
```
The Multiplication Table:
1	2	3
2	4	6
3	6	9

The 5-th smallest number is 3 (1, 2, 2, 3, 3).
```
Example 2:
```
Input: m = 2, n = 3, k = 6
Output: 
```
Explanation: 
```
The Multiplication Table:
1	2	3
2	4	6

The 6-th smallest number is 6 (1, 2, 2, 3, 4, 6).
```
Note:
```
The m and n will be in the range [1, 30000].
The k will be in the range [1, m * n]
```
```c++
class Solution {
public:
    static bool possible(int m, int n, int mid, int k){
        int sum = 0;
        for(int i = 1; i <= m; ++ i){
            sum += min(n, mid / i);
        }
        return sum >= k;
    }
    
    int findKthNumber(int m, int n, int k) {
        if(m > n)
            std::swap(m, n);
        int a = 1, b = m * n;
        while(a < b){
            int mid = (a + b) >> 1;
            if(possible(m, n, mid, k))
                b = mid;
            else
                a = mid + 1;
        }
        return a;
    }
};
```
```c++
class Solution {
public:
    static bool possible(int m, int n, int mid, int k){
        int sum = 0;
        for(int i = 1; i <= m; ++ i){
            sum += min(n, mid / i);
        }
        return sum >= k;
    }
    
    int findKthNumber(int m, int n, int k) {
        if(m > n)
            std::swap(m, n);
        int a = 1, b = m * n;
        while(a < b){
            int mid = (a + b) >> 1;
            if(possible(m, n, mid, k))
                b = mid;
            else
                a = mid + 1;
        }
        return a;
    }
};
```
```c++
class Solution {
public:
    int findKthNumber(int m, int n, int k) {
        if(m*n <= 0 || k <= 0){
            return -1;
        }
        
        if(m > n){
            swap(m, n);
        }
        
        int l = 1, r = m*n;
        while(l < r){
            int mid = (l + r)>>1;
            int cnt = 0;
            for(int i = 1; i <= m; ++ i) {
                int delta = min(mid/i, n);
                cnt += delta;
            }
            if(cnt < k){
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        
        return l;
    }
};
```