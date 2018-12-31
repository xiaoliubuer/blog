---
layout: post
title:  "* 305. Number of Islands II"
date: 2018-12-28 05:38:23 -0400
categories: articles
---
A 2d grid map of m rows and n columns is initially filled with water. We may perform an addLand operation which turns the water at position (row, col) into a land. Given a list of positions to operate, count the number of islands after each addLand operation. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Example:

Input: m = 3, n = 3, positions = [[0,0], [0,1], [1,2], [2,1]]
Output: [1,1,2,3]
Explanation:

Initially, the 2d grid grid is filled with water. (Assume 0 represents water and 1 represents land).
```
0 0 0
0 0 0
0 0 0
```
Operation #1: addLand(0, 0) turns the water at grid[0][0] into a land.
```
1 0 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #2: addLand(0, 1) turns the water at grid[0][1] into a land.
```
1 1 0
0 0 0   Number of islands = 1
0 0 0
```
Operation #3: addLand(1, 2) turns the water at grid[1][2] into a land.
```
1 1 0
0 0 1   Number of islands = 2
0 0 0
```
Operation #4: addLand(2, 1) turns the water at grid[2][1] into a land.
```
1 1 0
0 0 1   Number of islands = 3
0 1 0
```
Follow up:
Can you do it in time complexity O(k log mn), where k is the length of the positions?

# 题意
给一个 m * n 的矩阵代表陆地或水。0是水，1是陆地。给一个list的操作，比如：positions = [[0,0], [0,1], [1,2], [2,1]]，那么计算每一个操作以后这个地方的陆地的数量，输出一个list。

其实简单来说就是遍历每一个，操作，生成新的地图之后对新的地图进行find component的操作