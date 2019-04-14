---
layout: post
title:  "202. Happy Number"
date: 2019-04-07 16:25:00 -0400
categories: articles
---
Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. Those numbers for which this process ends in 1 are happy numbers.

Example: 
```
Input: 19
Output: true
Explanation: 
12 + 92 = 82
82 + 22 = 68
62 + 82 = 100
12 + 02 + 02 = 1
```

```c++
// Accepted!!
class Solution {
public:
    unordered_set<int> buffer;
    bool isHappy(int n) {
        if (n == 1) return true;
        if ( buffer.find(n) != buffer.end()) return false;
        int newN = 0;
        buffer.insert(n);
        while ( n > 0 ){
            int m = n % 10;
            n /= 10;
            newN += m*m;
        }
        if (isHappy(newN)) return true;
        return false;
    }
};
```