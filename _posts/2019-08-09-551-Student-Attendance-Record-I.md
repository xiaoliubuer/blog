---
layout: post
title:  "551. Student Attendance Record I"
date: 2019-08-09 21:25:00 -0400
categories: articles
---
You are given a string representing an attendance record for a student. The record only contains the following three characters:
```
'A' : Absent.
'L' : Late.
'P' : Present.
```
A student could be rewarded if his attendance record doesn't contain more than one 'A' (absent) or more than two continuous 'L' (late).

You need to return whether the student could be rewarded according to his attendance record.

Example 1:
```
Input: "PPALLP"
Output: True
```
Example 2:
```
Input: "PPALLL"
Output: False
```
```c++
class Solution {
public:
    bool checkRecord(string s) {
        if(s.size() < 2) return true;
        int i1 = 0, i2 = 0;
        for(int i=0; i<s.size(); i++){
            if(s[i] == 'A'){
                i1++, i2=0;
                if(i1 > 1) return false;
            } 
            else if(s[i] == 'L'){
                i2++;
                if(i2 > 2) return false;
            }
            else{
                i2 = 0;
            }
        }
        return true;
    }
};
```