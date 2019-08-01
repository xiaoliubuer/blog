---
layout: post
title:  "367. Valid Perfect Square"
date: 2019-07-31 20:18:00 -0400
categories: articles
---
Given a positive integer num, write a function which returns True if num is a perfect square else False.

Note: Do not use any built-in library function such as sqrt.

Example 1:
```
Input: 16
Output: true
```
Example 2:
```
Input: 14
Output: false
```

```c++
class Solution {
public:
    bool isPerfectSquare(int num) {
        if(num == 0) return 1;
        long long hi = 1, lo = 1;
        while(hi * hi < num) hi <<= 1;
        lo = hi >> 1;
        while(lo < hi) {
            long long mid = (hi + lo) / 2;
            if(mid * mid >= num) hi = mid;
            else lo = mid + 1;
        }
        return hi * hi == num;
    }
};
```
```c++
class Solution {
public:
    bool isPerfectSquare(int num) {
        long long low = 0, high = num;
        while (low <= high) {
            long long mid = low + (high - mid) / 2;
            long long sq = mid * mid;
            if (num == sq) {
                return true;
            } else if (num < sq) {
                high = mid - 1;
            } else {
                low = mid + 1;
            }
        }
        return false;
    }
};
```