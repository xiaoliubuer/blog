---
layout: post
title:  "483. Smallest Good Base"
date: 2019-08-06 20:04:00 -0400
categories: articles
---
For an integer n, we call k>=2 a good base of n, if all digits of n base k are 1.

Now given a string representing n, you should return the smallest good base of n in string format.

Example 1:
```
Input: "13"
Output: "3"
Explanation: 13 base 3 is 111.
```

Example 2:
```
Input: "4681"
Output: "8"
Explanation: 4681 base 8 is 11111.
```

Example 3:
```
Input: "1000000000000000000"
Output: "999999999999999999"
Explanation: 1000000000000000000 base 999999999999999999 is 11.
```

Note:
```
The range of n is [3, 10^18].
The string representing n is always valid and will not have leading zeros.
```

```c++
class Solution {
    long long f(long long b, int n)
    {
        long long k = 1;
        for (int i = 1; i < n; ++i)
            k = k*b + 1;
        return k;
    }
    long long solve(long long k, int n)
    {
        long long bl = pow(k, 1./n), bh =  pow(k, 1./(n - 1) );
        if (n == 2) bh = k;
        while(bl <= bh)
        {
            long long mid = bl + (bh - bl) /2;
            long long t = f(mid, n);
            if (t > k)
                bh = mid - 1;
            else if (t < k)
                bl = mid + 1;
            else
                return mid;
        }
        return -1;
    }
public:
    string smallestGoodBase(string s) {
        long long k = stoll(s);
        for (int i = log2(k) + 1; i >= 3; --i )
        {
            long long b = solve(k, i);
            if (b != -1)
                return to_string(b);
        }
        return to_string(k - 1);
        
    }
};
```