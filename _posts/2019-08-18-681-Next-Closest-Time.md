---
layout: post
title:  "681. Next Closest Time"
date: 2019-08-18 17:33:00 -0400
categories: articles
---
Given a time represented in the format "HH:MM", form the next closest time by reusing the current digits. There is no limit on how many times a digit can be reused.

You may assume the given input string is always valid. For example, "01:34", "12:09" are all valid. "1:34", "12:9" are all invalid.

Example 1:
```
Input: "19:34"
Output: "19:39"
```
Explanation: 
```
The next closest time choosing from digits 1, 9, 3, 4, is 19:39, which occurs 5 minutes later.  It is not 19:33, because this occurs 23 hours and 59 minutes later.
```
Example 2:
```
Input: "23:59"
Output: "22:22"
```
Explanation:
```
The next closest time choosing from digits 2, 3, 5, 9, is 22:22. It may be assumed that the returned time is next day's time since it is smaller than the input time numerically.
```
```c++
class Solution {
public:
    string nextClosestTime(string time) {
    	int mins[4] = {600, 60, 10, 1};
        size_t colon = time.find(':');
        int cur = stoi(time.substr(0, colon)) * 60 + stoi(time.substr(colon+1));
        string next = "0000";

        for (int i = 1, d = 0; i <= 1440 && d < 4; i++){
        	int m = (cur + i) % 1440;
        	for (d = 0; d < 4; d++){
        		next[d] = '0' + m / mins[d];
        		m %= mins[d];
        		if ( time.find(next[d]) == string::npos)
        			break;
        	}
        }
        return next.substr(0,2) + ':' + next.substr(2,2);
    }
};
```
```c++
class Solution {
public:
    string nextClosestTime(string time) {
        set<int> pool;
        pool.insert(time[0]-'0');
        pool.insert(time[1]-'0');
        pool.insert(time[3]-'0');
        pool.insert(time[4]-'0');
        
        string res=time;
        
		// try to make the time a bit larger starting from the last digit
        auto fourth = pool.upper_bound(time[4]-'0');
        if (fourth!=pool.end()) {
            res[4]=*fourth+'0';
            return res;
        }
        
        auto third = pool.upper_bound(time[3]-'0');
        if (third!=pool.end()&&*third<6) {
            res[3]=*third+'0';
            res[4]=*pool.begin()+'0';
            return res;
        }
        
        auto second = pool.upper_bound(time[1]-'0');
        if (second!=pool.end()) {
            if (time[0]-'0'<2||*second<4) {
                res[1]=*second+'0';
                res[3]=*pool.begin()+'0';
                res[4]=*pool.begin()+'0';
                return res;
            }
        }
        
        auto first = pool.upper_bound(time[0]-'0');
        if (first!=pool.end()&&*first<3) {
            res[0]=*first+'0';
            res[1]=*pool.begin()+'0';
            res[3]=*pool.begin()+'0';
            res[4]=*pool.begin()+'0';
            return res;
        }
        
		// If all previous tries failed , then the answer is the earliest time of next day
        res[0]=*pool.begin()+'0';
        res[1]=*pool.begin()+'0';
        res[3]=*pool.begin()+'0';
        res[4]=*pool.begin()+'0';
        
        return res;
    }
};
```