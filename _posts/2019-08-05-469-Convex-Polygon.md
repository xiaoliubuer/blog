---
layout: post
title:  "469. Convex Polygon"
date: 2019-08-05 20:04:00 -0400
categories: articles
---
Given a list of points that form a polygon when joined sequentially, find if this polygon is convex (Convex polygon definition).

Note:

There are at least 3 and at most 10,000 points.
Coordinates are in the range -10,000 to 10,000.
You may assume the polygon formed by given points is always a simple polygon (Simple polygon definition). In other words, we ensure that exactly two edges intersect at each vertex, and that edges otherwise don't intersect each other.
 

Example 1:
```
[[0,0],[0,1],[1,1],[1,0]]

Answer: True
```
Explanation:
```
Example 2:

[[0,0],[0,10],[10,10],[10,0],[5,5]]

Answer: False
```
Explanation:

```c++
class Solution {
public:
    int cross(vector<int>& a, vector<int>& b, vector<int>& c) {
        return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0]);
    }
    bool isConvex(vector<vector<int>>& p) {
        int n = p.size();
        if (n < 3) return false;
        p.push_back(p[0]);
        p.push_back(p[1]);
        int sign = 0;
        for (int i = 0; i < n; ++i) {
            int cur = cross(p[i], p[i + 1], p[i + 2]);
            if (cur == 0) continue;
            if (!sign) sign = cur / abs(cur);
            if (cur * sign < 0) return false;
        }
        return sign != 0;
    }
};
```
```c++
class Solution {
public:
    bool isConvex(vector<vector<int>>& p) {
      for (int i=0, pos=0, neg=0, n=p.size(); i < n; ++i) {
        long det = det2({p[i], p[(i+1)%n], p[(i+2)%n]});
        if ((pos|=(det>0))*(neg|=(det<0))) return false;
      }    
      return true;
    }
    // determinant of 2x2 matrix [A1-A0, A2-A0]
    long det2(const vector<vector<int>>& A) {
    	return (A[1][0]-A[0][0])*(A[2][1]-A[0][1]) - (A[1][1]-A[0][1])*(A[2][0]-A[0][0]);
    }
};
```
```c++
class Solution {
public:
     bool isConvex(vector<vector<int>>& points) {
        if (points.size() < 3) return false;
        int res = -1;
        for (int i = 0; i < points.size(); i++) {
            auto pre = i == 0 ? points.back() : points[i - 1];
            auto after = i == points.size() - 1 ? points[0] : points[i + 1];
            int x1 = points[i][0] - pre[0], y1 = points[i][1] - pre[1];
            int x2 = after[0] - points[i][0], y2 = after[1] - points[i][1];
            int flag = x1 * y2 - x2 * y1;
            if (res == -1) {
                if (flag) res = flag > 0;
            }
            else if (res != flag > 0) return false;
        }
        return true;
    }
};
```