---
layout: post
title:  "258. Add Digits"
date: 2019-07-28 16:13:00 -0400
categories: articles
---
Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.

Example:
```
Input: 38
Output: 2 
Explanation: The process is like: 3 + 8 = 11, 1 + 1 = 2. 
             Since 2 has only one digit, return it.
```
Follow up:
Could you do it without any loop/recursion in O(1) runtime?

```c++
class Solution {
public:
    int addDigits(int num) {
        if (num <= 9) return num;
        int sum = 0;
        while (num) {
            sum += num % 10;
            num /= 10;
        }
        return addDigits(sum);
    }
};
```