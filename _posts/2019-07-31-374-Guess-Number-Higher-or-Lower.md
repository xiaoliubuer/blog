---
layout: post
title:  "374. Guess Number Higher or Lower"
date: 2019-07-31 20:35:00 -0400
categories: articles
---
We are playing the Guess Game. The game is as follows:

I pick a number from 1 to n. You have to guess which number I picked.

Every time you guess wrong, I'll tell you whether the number is higher or lower.

You call a pre-defined API guess(int num) which returns 3 possible results (-1, 1, or 0):

-1 : My number is lower
 1 : My number is higher
 0 : Congrats! You got it!
Example :
```
Input: n = 10, pick = 6
Output: 6
```


```c++
class Solution {
public:
    int guessNumber(int n) {
        if ( n == 0 ) return 0;
        int left = 1, right = n;
        while ( left + 1 < right ) {
            int mid = left + ( right - left )/2;
            if (guess(mid) == 0) return mid;
            else if ( guess(mid) == -1 )
                right = mid;
            else
                left = mid;
        }
        if (guess(left) == 0 ) return left;
        if (guess(right)== 0 ) return right;
        return 0;
    }
};
```