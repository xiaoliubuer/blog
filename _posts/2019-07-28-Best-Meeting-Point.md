---
layout: post
title:  "296. Best Meeting Point"
date: 2019-07-28 20:14:00 -0400
categories: articles
---

A group of two or more people wants to meet and minimize the total travel distance. You are given a 2D grid of values 0 or 1, where each 1 marks the home of someone in the group. The distance is calculated using Manhattan Distance, where distance 
```
(p1, p2) = |p2.x - p1.x| + |p2.y - p1.y|.
```

Example:

```
Input: 

1 - 0 - 0 - 0 - 1
|   |   |   |   |
0 - 0 - 0 - 0 - 0
|   |   |   |   |
0 - 0 - 1 - 0 - 0

Output: 6 
```

Explanation: 
```
Given three people living at (0,0), (0,4), and (2,2):
             The point (0,2) is an ideal meeting point, as the total travel distance 
             of 2+2+2=6 is minimal. So return 6.
```

```c++
class Solution {
public:
int minTotalDistance(vector<vector<int>>& grid) {
    const int row = grid.size();
    if (0 == row) return 0;
    const int col = grid[0].size();
    int total = 0;
    vector<int> posR, posC;
    for (int i = 0; i < row; ++i) 
     for (int j = 0; j < col; ++j) {
         if (grid[i][j] == 1) {
             posR.emplace_back(i);
             posC.emplace_back(j);
         }
     }
    int med1 = posR[posR.size() / 2];
    nth_element(posC.begin(), posC.begin() +  posC.size() / 2, posC.end());
    int med2 = posC[posC.size() / 2];
    for (auto pos1 : posR) total += abs(pos1 - med1);
    for (auto pos2 : posC) total += abs(pos2 - med2);
    return total;
}
};
```