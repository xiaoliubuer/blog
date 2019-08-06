---
layout: post
title:  "447. Number of Boomerangs"
date: 2019-08-02 20:41:00 -0400
categories: articles
---
Given n points in the plane that are all pairwise distinct, a "boomerang" is a tuple of points (i, j, k) such that the distance between i and j equals the distance between i and k (the order of the tuple matters).

Find the number of boomerangs. You may assume that n will be at most 500 and coordinates of points are all in the range [-10000, 10000] (inclusive).

Example:
```
Input:
[[0,0],[1,0],[2,0]]

Output:
2
```
Explanation:
The two boomerangs are [[1,0],[0,0],[2,0]] and [[1,0],[2,0],[0,0]]

```c++
class Solution {
public:
    int getDist(int a, int b, int c, int d) {
        return (a-c)*(a-c) + (b-d)*(b-d);
    }
    int numberOfBoomerangs(vector<vector<int>>& points) {
        int ans = 0;
        if(!points.size()) return ans;
        for(int i = 0; i < points.size(); i++) {
            unordered_map<int, int> m;
            for(int j = 0; j < points.size(); j++) {
                 if(j != i) m[getDist(points[i][0], points[i][1], points[j][0], points[j][1])]++;
            }
            for(auto &p: m) {
                if(p.second >= 2) ans += (p.second*(p.second-1));
            }
        }
        return ans;
    }
};

```