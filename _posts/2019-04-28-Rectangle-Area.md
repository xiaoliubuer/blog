---
layout: post
title:  "223. Rectangle Area"
date: 2019-04-28 15:49:00 -0400
categories: articles
--- 
Find the total area covered by two rectilinear rectangles in a 2D plane.

Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.

Rectangle Area

Example:
```
Input: A = -3, B = 0, C = 3, D = 4, E = 0, F = -1, G = 9, H = 2
Output: 45
```
Note:

Assume that the total area is never beyond the maximum possible value of int.
```c++
// Better solution!!
class Solution {
public:
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int left = max(A, E), right = max(min(C, G), left);
        int buttom = max(B, F), top = max(min(D, H), buttom);
        return abs(C-A)*abs(D-B) - (right - left)*(top - buttom) + abs(G-E)*abs(H-F);
        
    }
};
```
```c++
// Signed integer overflow
class Solution {
public:
    
    int helper (int a1, int a2, int b1, int b2){
        if ( min(a1, a2) >= max(b1,b2) || max(a1, a2) <= min(b1, b2) ) return 0;
        else {
            if ( a1 < b1 && a2 > b2 ) return abs(b1 - b2);
            if ( b1 < a1 && b2 > a2 ) return abs(a1 - a2);
            if ( a2 < b2 ) return abs(a2 - b1);
            if ( b1 < a1 ) return abs(a1 - b2);
        }
        return 0;
    }
    int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
        int overlap =  helper(A, C, E, G) * helper(B, D, F, H);
        abs(C - A) * abs(D-B) - overlap + abs(G - E) * abs(H - F);
    }
};
```