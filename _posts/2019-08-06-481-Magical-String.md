---
layout: post
title:  "481. Magical String"
date: 2019-08-06 19:42:00 -0400
categories: articles
---
A magical string S consists of only '1' and '2' and obeys the following rules:

The string S is magical because concatenating the number of contiguous occurrences of characters '1' and '2' generates the string S itself.

The first few elements of string S is the following: S = "1221121221221121122……"

If we group the consecutive '1's and '2's in S, it will be:

1 22 11 2 1 22 1 22 11 2 11 22 ......

and the occurrences of '1's or '2's in each group are:

1 2	2 1 1 2 1 2 2 1 2 2 ......

You can see that the occurrence sequence above is the S itself.

Given an integer N as input, return the number of '1's in the first N number in the magical string S.

Note: N will not exceed 100,000.

Example 1:
```
Input: 6
Output: 3
Explanation: The first 6 elements of magical string S is "12211" and it contains three 1's, so return 3.
```
```c++
class Solution {
public:
    /* 
    Say x is the number that S[i] represents. If i was over the size of S, set x equals to i. 
    Then start the loop. When i is odd, append '1' x times, otherwise append '2' x times. 
    In the end, count how many '1' in S from S.begin() to S.begin() + n + 1. 

    如何生成这个magical string (Kolakoski Sequence)?
    设 x 为s[i]上的"数字", 如果i越界, 则 x = i;
    如果i是奇数, 则append x 个 1
    如果i是偶数, 则append x 个 2
    最后数一共多少个1.
    trick: s可以定义为" ", 这样index比较好看, 然后最后记得是s.begin() 到 s.being() + n + 1的范围内.
    */
    int magicalString(int n) {
        string s = " ";
        for (int i = 1, x = 0; i <= n; ++i) {
            if (i >= s.size()) { x = i; }
            else { x = s[i] - '0'; }
            
            if (i & 1) {
                s.append(x, '1');
            } else {
                s.append(x, '2');
            }
        }
        return count(s.begin(), s.begin() + n + 1, '1');
    }
};
```