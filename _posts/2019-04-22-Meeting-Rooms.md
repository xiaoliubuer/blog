---
layout: post
title:  "252. Meeting Rooms"
date: 2019-04-22 21:31:00 -0400
categories: articles
---
Given an array of meeting time intervals consisting of start and end times 
```
[[s1,e1],[s2,e2],...] (si < ei)
```
determine if a person could attend all meetings.

Example 1:
```
Input: [[0,30],[5,10],[15,20]]
Output: false
```
Example 2:
```
Input: [[7,10],[2,4]]
Output: true
```

NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.


```c++
//Accepted!
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int>& A, vector<int>& B){
            return A[0] < B[0];
        });
        
        for ( int i = 0 ; i + 1 < intervals.size(); i++ ){
            if ( intervals[i][1] > intervals[i+1][0]) return false;
        }
        return true;
    }
};
```

