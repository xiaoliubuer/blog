---
layout: post
title:  "470. Implement Rand10() Using Rand7()"
date: 2019-08-05 20:06:00 -0400
categories: articles
---
Given a function rand7 which generates a uniform random integer in the range 1 to 7, write a function rand10 which generates a uniform random integer in the range 1 to 10.

Do NOT use system's Math.random().
Example 1:
```
Input: 1
Output: [7]
```
Example 2:
```
Input: 2
Output: [8,4]
```
Example 3:
```
Input: 3
Output: [8,1,10]
```
Note:

rand7 is predefined.
Each testcase has one argument: n, the number of times that rand10 is called.
 

Follow up:

What is the expected value for the number of calls to rand7() function?
Could you minimize the number of calls to rand7()?



```c++
class Solution {
public:
    int rand10() {
        int rand40 = 40;
        while (rand40 >= 40) {
            rand40 = (rand7() - 1) * 7 + rand7() - 1;
        }
        return rand40 % 10 + 1;
    }
};
```
```c++
class Solution {
public:
    int rand10() {
        int res=50;
        
        while(res>40){
            res=rand7()+(rand7()-1)*7;
        }
        
        return (res-1)%10+1;
    }
};
```
```c++
class Solution {
public:
    int rand10() {
        int t = rand7();
        while (t == 4)
            t = rand7();
        int res = t < 4 ? 0 : 5;
        t = rand7();
        while (t > 5)
            t = rand7();
        return res + t;
    }
};
```
```c++
class Solution {
public:
    int rand10() {
        int rand40 = 40;
        while (rand40 >= 40) {
            rand40 = (rand7() - 1) * 7 + rand7() - 1;
        }
        return rand40 % 10 + 1;
    }
};
```