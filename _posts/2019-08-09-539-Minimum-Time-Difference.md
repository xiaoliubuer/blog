---
layout: post
title:  "539. Minimum Time Difference"
date: 2019-08-09 20:44:00 -0400
categories: articles
---
Given a list of 24-hour clock time points in "Hour:Minutes" format, find the minimum minutes difference between any two time points in the list.
Example 1:
```
Input: ["23:59","00:00"]
Output: 1
```
Note:
```
The number of time points in the given list is at least 2 and won't exceed 20000.
The input time is legal and ranges from 00:00 to 23:59.
```
```c++
class Solution {
public:
    int toMinutes(const string& str){
        return atoi(str.substr(0,2).c_str())*60 + atoi(str.substr(3,2).c_str());
    }
    int findMinDifference(vector<string>& timePoints) {
        vector<int> minutes;
        for (auto & x: timePoints) {
            minutes.push_back(toMinutes(x));
        }
        sort(minutes.begin(),minutes.end());
        int ret = minutes[1] - minutes[0];
        for(int i = 1; i< minutes.size()-1;i++) {
            ret = min(ret,minutes[i+1] - minutes[i]);
        }
        int tmp = (1440 - minutes.back()) + minutes.front();
        return min(tmp,ret);
    }
};
```