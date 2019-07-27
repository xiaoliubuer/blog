---
layout: post
title:  "204. Count Primes"
date: 2019-07-27 18:19:00 -0400
categories: articles
---
Count the number of prime numbers less than a non-negative number, n.
Example:
```
Input: 10
Output: 4
Explanation: There are 4 prime numbers less than 10, they are 2, 3, 5, 7.
```
```c++
class Solution {
public:
int countPrimes(int n) {
    if(--n < 2) return 0;
    int m = (n + 1)/2, count = m, k, u = (sqrt(n) - 1)/2;
    bool notPrime[m] = {0};

    for(int i = 1; i <= u;i++)
        if(!notPrime[i])
          for(k = (i+ 1)*2*i; k < m;k += i*2 + 1)
              if (!notPrime[k])
              {
                  notPrime[k] = true;
                  count--;
              }
    return count;
}
};
```