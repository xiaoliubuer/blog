---
layout: post
title:  "573. Squirrel Simulation"
date: 2019-08-11 17:22:00 -0400
categories: articles
---

There's a tree, a squirrel, and several nuts. Positions are represented by the cells in a 2D grid. Your goal is to find the minimal distance for the squirrel to collect all the nuts and put them under the tree one by one. The squirrel can only take at most one nut at one time and can move in four directions - up, down, left and right, to the adjacent cell. The distance is represented by the number of moves.
Example 1:
```
Input: 
Height : 5
Width : 7
Tree position : [2,2]
Squirrel : [4,4]
Nuts : [[3,0], [2,5]]
Output: 12
```
Explanation:
​​​​​
Note:
```
All given positions won't overlap.
The squirrel can take at most one nut at one time.
The given positions of nuts have no order.
Height and width are positive integers. 3 <= height * width <= 10,000.
The given positions contain at least one nut, only one tree and one squirrel.
```
```c++
class Solution {
public:
    int minDistance(int height, int width, vector<int>& tree, vector<int>& squirrel, vector<vector<int>>& nuts) {
        
        int n = nuts.size();
        vector<int> v(n);
        long sum = 0;
        int max_diff = INT_MIN;
        for(int i = 0; i < n; ++i) {
            v[i] = distance(nuts[i], tree)*2;
            max_diff = max(max_diff, v[i]/2 - distance(squirrel, nuts[i]));
            sum += v[i];
        }
        return sum - max_diff;
    }
    
    int distance(vector<int>& v1, vector<int>& v2) {
        return abs(v1[0]-v2[0]) + abs(v1[1]-v2[1]); 
    }
};
```
```c++
class Solution {
public:
    int minDistance(int height, int width, vector<int>& tree, vector<int>& squirrel, vector<vector<int>>& nuts) {
        int dis = 0, diff = INT_MIN;
        for(int i = 0 ; i < nuts.size() ; i++){
            int dis_s = distance_calculate(nuts[i],squirrel);
            int dis_t = distance_calculate(nuts[i],tree);
            diff = max(diff,dis_t-dis_s);
            dis += 2*dis_t;
        }
        return dis - diff;
    }
    int distance_calculate(vector<int>& n1, vector<int>& n2){
        return abs(n1[0]-n2[0]) + abs(n1[1]-n2[1]);
    }
};
```