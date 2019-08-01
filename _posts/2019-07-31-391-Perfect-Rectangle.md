---
layout: post
title:  "391. Perfect Rectangle"
date: 2019-07-31 21:54:00 -0400
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

```c++
class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
    set<pair<int, int>> corners;
    int area = 0;
    for (const auto& rect : rectangles) {            
      pair<int, int> p1{rect[0], rect[1]};
      pair<int, int> p2{rect[2], rect[1]};
      pair<int, int> p3{rect[2], rect[3]};
      pair<int, int> p4{rect[0], rect[3]};
      for (const auto& p : {p1, p2, p3, p4}) {        
        const auto& ret = corners.insert(p);
        if (!ret.second) corners.erase(ret.first);
      }
      area += (p3.first - p1.first) * (p3.second - p1.second);
    }
    if (corners.size() != 4) return false;
    const auto& p1 = *begin(corners);
    const auto& p3 = *rbegin(corners);    
    return area == (p3.first - p1.first) * (p3.second - p1.second);
    }
};
```
```c++
class Solution {
public:
    bool isRectangleCover(vector<vector<int>>& rectangles) {
        set<pair<int,int>> kPoint;
        int area=0;
        for(const auto& pp :rectangles){
            pair<int,int> p1{pp[0],pp[1]};
            pair<int,int> p2{pp[2],pp[1]};
            pair<int,int> p3{pp[2],pp[3]};
            pair<int,int> p4{pp[0],pp[3]};

            for(const auto& p:{p1,p2,p3,p4}){
                const auto& ret=kPoint.insert(p);//ret.sec表示是否有新插入非重值
                if(!ret.second) kPoint.erase(ret.first);//若sec值=0，说明是没有新值则去除重复点
            }
            area+=(p3.first-p1.first)*(p3.second-p1.second);
        }
        if(kPoint.size()!=4) return false;
        const auto& p11=*begin(kPoint);
        const auto& p33=*rbegin(kPoint);
        return area==(p33.first-p11.first)*(p33.second-p11.second);
    }
};
```
```c++
class Solution{
public:
    bool isRectangleCover(vector<vector<int>>& rectangles){
        set<pair<int,int>> corners; //先x排序后y排序， 使得左下角在第一个， 右上角在最后一个
        int area = 0;
        for(const auto& rect: rectangles){
            //按照逆时针方向取角
            pair<int,int> p1{rect[0], rect[1]};// 左下角
            pair<int,int> p2{rect[2], rect[1]}; //右下角
            pair<int,int> p3{rect[2], rect[3]}; //右上角
            pair<int,int> p4{rect[0], rect[3]}; //左上角
            for(const auto& p: {p1, p2, p3, p4}){
                //先插入到set,如果set里面存在的话则它的second会是true.
                const auto& ret = corners.insert(p);
                //出现偶数次则删除， 最后每个顶点都是出现奇数次的
                if(!ret.second) corners.erase(ret.first);
            }
            area += (p3.first - p1.first) * (p3.second - p1.second);
        }
        if(corners.size() != 4) return false;
        const auto& p1 = *begin(corners);
        const auto& p3 = *rbegin(corners);
        return area == (p3.first - p1.first) * (p3.second - p1.second);
    }
        
};
```