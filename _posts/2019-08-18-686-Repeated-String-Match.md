---
layout: post
title:  "686. Repeated String Match"
date: 2019-08-18 17:49:00 -0400
categories: articles
---
Given two strings A and B, find the minimum number of times A has to be repeated such that B is a substring of it. If no such solution, return -1.

For example, with A = "abcd" and B = "cdabcdab".

Return 3, because by repeating A three times (“abcdabcdabcd”), B is a substring of it; and B is not a substring of A repeated two times ("abcdabcd").

Note:
The length of A and B will be between 1 and 10000.

```c++
class Solution {
public:
    int repeatedStringMatch(string A, string B) {
  		string rep = A;
  		for (int i = 1; i <= B.length()/A.length() + 2; ++i, rep+= A){
  			if (rep.find(B) != string::npos){
  				return i;
  			}
  		}
  		return -1;
    }
};
```
```c++
class Solution {
public:
    int repeatedStringMatch(string a, string b) {
        string as = a;
        for (int rep = 1; rep <= b.length() / a.length() + 2; rep++, as += a)
            if (as.find(b) != string::npos) return rep;
        return -1;
    }
};
```