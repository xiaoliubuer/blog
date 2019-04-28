---
layout: post
title:  "252. Meeting Rooms II"
date: 2019-04-22 22:44:00 -0400
categories: articles
---

Given an array of meeting time intervals consisting of start and end times [[s1,e1],[s2,e2],...] (si < ei), find the minimum number of conference rooms required.

Example 1:

Input: [[0, 30],[5, 10],[15, 20]]
Output: 2
Example 2:

Input: [[7,10],[2,4]]
Output: 1
```c++
//Accepted!!
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        vector<int> startTime;
        vector<int> endTime;
        
        for (int i = 0; i < intervals.size(); i++) {
            startTime.push_back(intervals[i][0]);
            endTime.push_back(intervals[i][1]);
        }
        
        sort(startTime.begin(), startTime.end());
        sort(endTime.begin(), endTime.end());
        
        int overlap = 0;
        int res = 0;
        for (int i = 0, j = 0; i < startTime.size(); ){
            if ( startTime[i] < endTime[j] ){
                overlap++;
                res = max(res, overlap);
                i++;
            }
            else{
                overlap--;
                j++;
            }
        }
        return res;
    }
};
```
```c++
// Accepted!
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        if ( intervals.size() < 2) return intervals.size();
        int res = 0;
        int last = 0;
        unordered_map<int, int> mymap;
        for (auto i : intervals) {
            mymap[i[0]]++;
            mymap[i[1]]--;
            last = max(last, max(i[0], i[1]));
        }
        int count = 0;
        for (int i = 0; i <= last; i++){
            count += mymap[i];
            res = max(res, count);
        }
        return res;
    }
};
```