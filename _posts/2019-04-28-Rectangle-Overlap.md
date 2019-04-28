---
layout: post
title:  "836. Rectangle Overlap"
date: 2019-04-28 15:20:00 -0400
categories: articles
---
A rectangle is represented as a list [x1, y1, x2, y2], where (x1, y1) are the coordinates of its bottom-left corner, and (x2, y2) are the coordinates of its top-right corner.

Two rectangles overlap if the area of their intersection is positive.  To be clear, two rectangles that only touch at the corner or edges do not overlap.

Given two (axis-aligned) rectangles, return whether they overlap.

Example 1:
```
Input: rec1 = [0,0,2,2], rec2 = [1,1,3,3]
Output: true
```
Example 2:
```
Input: rec1 = [0,0,1,1], rec2 = [1,0,2,1]
Output: false
```
Notes:
```
Both rectangles rec1 and rec2 are lists of 4 integers.
All coordinates in rectangles will be between -10^9 and 10^9.
```
```c++
// Accepted!! 
// TC: O(n)
// SC: O(1)
class Solution {
public:
    bool helper(int a1, int a2, int b1, int b2){
        if ( min(a1, a2) >= max(b1,b2) || max(a1, a2) <= min(b1,b2)) return false;
        return true;
    }
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        return helper(rec1[0], rec1[2], rec2[0], rec2[2]) && helper(rec1[1], rec1[3], rec2[1], rec2[3]);
    }
};
```