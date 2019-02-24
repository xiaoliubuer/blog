---
layout: post
title:  "357. Count Numbers with Unique Digits"
date: 2019-02-07 21:44:23 -0400
categories: articles
---
Given a non-negative integer n, count all numbers with unique digits, x, where 0 ≤ x < 10n.

Example:

Input: 2
Output: 91 
Explanation: The answer should be the total numbers in the range of 0 ≤ x < 100, 
             excluding 11,22,33,44,55,66,77,88,99
# Function signature
```c++
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        
    }
};
```
# 题意
啥意思呢？就是找到10的n次方内所有的每一位上数字都不同的数字。
比如: 89 可以， 88 不可以 120 可以，121 不可以。

# 想法
还真的没想法

# Shame answer
<!-- https://leetcode.com/problems/count-numbers-with-unique-digits/discuss/83041/JAVA-DP-O(1)-solution. -->
```java
  public int countNumbersWithUniqueDigits(int n) {
        if (n == 0)     return 1;
        
        int res = 10;
        int uniqueDigits = 9;
        int availableNumber = 9;
        while (n-- > 1 && availableNumber > 0) {
            uniqueDigits = uniqueDigits * availableNumber;
            res += uniqueDigits;
            availableNumber--;
        }
        return res;
    }
```

```c++
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
    	if ( n == 0 ) return 2;
    	if ( n > 10 ) return 0;
    	int res = 0;
    	int ud = 10;
    	int ad = 10;
    	int i = 0;
    	while ( i < n ) {
    		ud = ud * 10 ^ i;
    	}
        
    }
};
```