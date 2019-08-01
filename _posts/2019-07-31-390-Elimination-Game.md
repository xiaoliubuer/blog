---
layout: post
title:  "390. Elimination Game"
date: 2019-07-31 21:50:00 -0400
categories: articles
---
There is a list of sorted integers from 1 to n. Starting from left to right, remove the first number and every other number afterward until you reach the end of the list.

Repeat the previous step again, but this time from right to left, remove the right most number and every other number from the remaining numbers.

We keep repeating the steps again, alternating left to right and right to left, until a single number remains.

Find the last number that remains starting with a list of length n.

Example:
```
Input:
n = 9,
1 2 3 4 5 6 7 8 9
2 4 6 8
2 6
6
```
Output:
6

```c++
class Solution {
public:
    int lastRemaining(int n) {
        return recursion(n, true);
    }
    int recursion(int n, bool isLeft) {
        if(n == 1) return n;
        if(!isLeft && (n % 2) == 0) {
            return recursion(n / 2, !isLeft) * 2 - 1;
        } else {
            return recursion(n / 2, !isLeft) * 2;
        }
    }
};
```
```c++
class Solution {
public:
    int lastRemaining(int n){
        if(n==1) return 1;
        else if(n<=5) return 2;
        else{
            if(n%2==1) return lastRemaining(n-1);
            else{
                int m=n/2;
                int a=lastRemaining(m/2);
                if(m%2==0) return 4*a-2;
                else return 4*a;
            }   
        }
    }
};
```
```c++
class Solution {
public:
    int f(int n, int first, int gap, bool left) {
        if (n == 1) return first;
        if (left) {
            return f(n / 2, first + gap, gap * 2, false);
        } else {
            return (n&1) ? f(n / 2, first + gap, gap * 2, true) : f(n / 2, first, gap * 2, true);
        }
    }
    int lastRemaining(int n) {
        return f(n, 1, 1, true);
    }
};
```
```c++
class Solution {
public:
    int lastRemaining( int n ) {
        return recursion( n, true );
    }
    int recursion( int n, bool isLeft ) {
        if( n == 1 ) return n;
        if( !isLeft && (n % 2) == 0 ) {
            return recursion( n / 2, !isLeft ) * 2 - 1;
        } else {
            return recursion( n / 2, !isLeft ) * 2;
        }
    }
};
```