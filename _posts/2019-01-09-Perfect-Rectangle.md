---
layout: post
title:  "** 391. Perfect Rectangle"
date: 2019-01-09 20:33:23 -0400
categories: articles
---
Given N axis-aligned rectangles where N > 0, determine if they all together form an exact cover of a rectangular region.

Each rectangle is represented as a bottom-left point and a top-right point. For example, a unit square is represented as [1,1,2,2]. (coordinate of bottom-left point is (1, 1) and top-right point is (2, 2)).


Example 1:
```
rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [3,2,4,4],
  [1,3,2,4],
  [2,3,3,4]
]

Return true. All 5 rectangles together form an exact cover of a rectangular region.
```
Example 2:
```
rectangles = [
  [1,1,2,3],
  [1,3,2,4],
  [3,1,4,2],
  [3,2,4,4]
]

Return false. Because there is a gap between the two rectangular regions.
```
Example 3:
```
rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [3,2,4,4]
]

Return false. Because there is a gap in the top center.
```
Example 4:
```
rectangles = [
  [1,1,3,3],
  [3,1,4,2],
  [1,3,2,4],
  [2,2,4,4]
]

Return false. Because two of the rectangles overlap with each other.
```
# Function signature
```c++
class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        
    }
};
```
# 题意
就是输入是一个list坐标，左下和右上的顶点坐标。[1,1,3,3], ld(1,1), tr(3,3), 确定一个square，问这所有顶点代表的所有square能不能形成一个square。它们不能有overlap。
# 想法
卧槽，完全没想法啊一上来。
形成正方形直观上用眼睛看才能判断出来，现在给的都是数字，如何能用数字中找到那么直观的办法？
用面积？？

