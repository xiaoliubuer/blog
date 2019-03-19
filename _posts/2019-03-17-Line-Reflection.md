---
layout: post
title:  "356. Line Reflection"
date: 2019-03-17 20:05:23 -0400
categories: articles
---



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