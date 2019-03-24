---
layout: post
title:  "796. Rotate String"
date: 2019-03-21 21:53:23 -0400
categories: articles
---

We are given two strings, A and B.

A shift on A consists of taking string A and moving the leftmost character to the rightmost position. For example, if A = 'abcde', then it will be 'bcdea' after one shift on A. Return True if and only if A can become B after some number of shifts on A.

```
Example 1:
Input: A = 'abcde', B = 'cdeab'
Output: true

Example 2:
Input: A = 'abcde', B = 'abced'
Output: false
```
Note:

A and B will have length at most 100.
Accepted
35,116
Submissions
71,934

```c++
class Solution {
public:
    bool rotateString(string A, string B) {
        
          if (A == B)
            return true;
        if (A.size() != B.size())
            return false;
        A.append(A);
        return A.find(B) < A.size();
        
    }
};
```
```c++
class Solution {
public:
    bool rotateString(string A, string B) {
        if ( A == B ) return true;
        int offset = A.size();
        int i = 1;
        while ( i < offset ) {
            std::rotate(A.begin(), A.begin() + 1, A.end());
            if (A == B) return true;
            i++;
        }
        return false;
    }
};
```

