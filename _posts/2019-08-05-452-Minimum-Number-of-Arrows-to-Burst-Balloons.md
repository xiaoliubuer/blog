---
layout: post
title:  "452. Minimum Number of Arrows to Burst Balloons"
date: 2019-08-05 18:55:00 -0400
categories: articles
---
There are a number of spherical balloons spread in two-dimensional space. For each balloon, provided input is the start and end coordinates of the horizontal diameter. Since it's horizontal, y-coordinates don't matter and hence the x-coordinates of start and end of the diameter suffice. Start is always smaller than end. There will be at most 104 balloons.

An arrow can be shot up exactly vertically from different points along the x-axis. A balloon with xstart and xend bursts by an arrow shot at x if xstart ≤ x ≤ xend. There is no limit to the number of arrows that can be shot. An arrow once shot keeps travelling up infinitely. The problem is to find the minimum number of arrows that must be shot to burst all balloons.

Example:
```
Input:
[[10,16], [2,8], [1,6], [7,12]]

Output:
2
```
Explanation:
One way is to shoot one arrow for example at x = 6 (bursting the balloons [2,8] and [1,6]) and another arrow at x = 11 (bursting the other two balloons).

```c++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0;

        auto cmp = [](vector<int>& a, vector<int>& b) { return a[1] < b[1]; };
        sort(intervals.begin(), intervals.end(), cmp);
        int end = intervals[0][1];
        int count = 1;
        for (int i = 1; i < intervals.size(); ++i) {
            if (intervals[i][0] > end) {
                end = intervals[i][1];
                count++;
            }
        }
        return count;        
    }
};
```

```c++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        
        sort(points.begin(),points.end(),sortBallons);
        
        int arrowsCount =0;
        
        for (int i=0; i<points.size(); i++)
        {
            int end = points[i][1];
            while (i < points.size()-1 && end >= points[i + 1][0])
                i++;
            arrowsCount++;
        }
        
        return arrowsCount;
        
    }
    
    static bool sortBallons(vector<int>& a, vector<int>& b)
    {
        return ( a[1]<b[1] );
    }
};
```
```c++
class Solution {
  public:
  int findMinArrowShots(vector<vector<int>>& points) {
    if (points.size() == 0) return 0;

    // sort by x_end
    sort(begin(points), end(points),
         [](const vector<int> &o1, const vector<int> &o2) {
      return (o1[1] < o2[1]);
    });

    int arrows = 1;
    int xStart, xEnd, firstEnd = points[0][1];
    for (auto p : points) {
      xStart = p[0];
      xEnd = p[1];
      // if the current balloon starts after the end of another one,
      // one needs one more arrow
      if (firstEnd < xStart) {
        arrows++;
        firstEnd = xEnd;
      }
    }
    return arrows;
  }
};
```

```c++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        if(points.empty()) return 0;
        sort(points.begin(), points.end(), [](const vector<int> &a, const vector<int> &b){return a[1] < b[1];});
        int size = points.size(), end = points[0][1], count = 1;
        for(int i = 1; i < size; ++i) {
            if(end < points[i][0]) {
                end = points[i][1];
                ++count;
            }
        }
        return count;
    }
};
```