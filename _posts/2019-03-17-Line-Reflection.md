---
layout: post
title:  "356. Line Reflection"
date: 2019-03-17 20:05:23 -0400
categories: articles
---

Given n points on a 2D plane, find if there is such a line parallel to y-axis that reflect the given points.

Example 1:
```
Input: [[1,1],[-1,1]]
Output: true
```
Example 2:
```
Input: [[1,1],[-1,-1]]
Output: false
```
Follow up:
Could you do better than O(n2) ?

# Answer
```c++
class Solution {
public:
    bool isReflected(vector<pair<int, int>>& points) {
        
        if (points.size() == 0) {
            return true;
        }
        
        std::set<std::pair<int, int>> xy_values;
                
        int x_min = points[0].first;
        int x_max = points[0].first;
        
        for (const auto& coord : points) {
            
            xy_values.insert(coord);
            
            x_min = std::min(x_min, coord.first);
            x_max = std::max(x_max, coord.first);        
        }
        
        double x_target = (static_cast<double>(x_min) + x_max) / 2;
        
        for (const auto& coord : xy_values) {
            
            double length = x_target - coord.first;
            
            if (xy_values.count({coord.first + 2 * length, coord.second}) == 0) {
                return false;
            }

        }
                
        return true;
    }
};
```

```c++
class Solution {
public:
    bool isReflected(vector<pair<int, int>>& points) {
        if (points.empty()) return true;
        int sum = 0;
        set<pair<double, int>> hash;
        for (auto p : points)
            if (hash.insert(p).second)
                sum += p.first;
        double avg = (double)sum / hash.size();
        for (auto p : points)
            if (p.first > avg && hash.count(make_pair(2*avg - p.first, p.second)) == 0)
                return false;
        return true;
    }
};
```