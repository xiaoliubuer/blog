---
layout: post
title:  "57. Insert Interval"
date: 2019-06-18 19:20:00 -0400
categories: articles
---
Given a set of non-overlapping intervals, insert a new interval into the intervals (merge if necessary).

You may assume that the intervals were initially sorted according to their start times.

Example 1:
```
Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]
```
Example 2:
```
Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
```
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].
NOTE: input types have been changed on April 15, 2019. Please reset to default code definition to get new method signature.

```c++
// 3 steps
class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int> newInterval) {
        vector<vector<int>> res;
        int index = 0;
        // Insert left part
        while(index < intervals.size() && intervals[index][1] < newInterval[0]){
            res.push_back(intervals[index++]);
        }

        // Merge
        while(index < intervals.size() && intervals[index][0] <= newInterval[1]){
            newInterval[0] = min(newInterval[0], intervals[index][0]);
            newInterval[1] = max(newInterval[1], intervals[index][1]);
            index++;
        }

        // Insert merged part
        res.push_back(newInterval);

        // Insert right part
        while(index < intervals.size()){
            res.push_back(intervals[index++]);
        }
        return res;
    }
};

// TC: O(N)
// SC: O(1)
```

```c++

class Solution {
public:
    vector<vector<int>> insert(vector<vector<int>>& intervals, vector<int>& newInterval) {
        vector<vector<int>> result;
        bool inserted = false;
        for (auto& v : intervals) {
            if (v.back() < newInterval.front())
                result.push_back(v);
            else if (v.front() > newInterval.back()) {
                if (!inserted) {
                    result.push_back(newInterval);
                    inserted = true;
                }
                result.push_back(v);
            } 
            else {
                newInterval.front() = min(v.front(), newInterval.front());
                newInterval.back() = max(v.back(), newInterval.back());
            }
        }
        if (!inserted)
            result.push_back(newInterval);
        return std::move(result);
    }
};
```