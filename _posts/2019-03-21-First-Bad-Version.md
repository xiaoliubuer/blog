---
layout: post
title:  "278. First Bad Version"
date: 2019-03-21 22:32:23 -0400
categories: articles
---



# FFFFFFFFFFFFFK!!!

binary search 最终返回的是踩在答案上的那个边界。
```c++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int left = 0; 
        int right = n;
        
        while ( left + 1 < right ) {
            int mid = left + ( right - left )/2;
            if ( isBadVersion(mid) ) right = mid;
            else left = mid;
        }
        return right;
    }
};


// In the last round, the situcation can be: good bad bad / good good bad 
// After the final round, it must be:  good(left) bad(right)
// Thus return right;
```