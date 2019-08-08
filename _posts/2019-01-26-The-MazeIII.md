---
layout: post
title:  "*** 499. The Maze III"
date: 2019-01-26 10:41:23 -0400
categories: articles
---
There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up (u), down (d), left (l) or right (r), but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction. There is also a hole in this maze. The ball will drop into the hole if it rolls on to the hole.

Given the ball position, the hole position and the maze, find out how the ball could drop into the hole by moving the shortest distance. The distance is defined by the number of empty spaces traveled by the ball from the start position (excluded) to the hole (included). Output the moving directions by using 'u', 'd', 'l' and 'r'. Since there could be several different shortest ways, you should output the lexicographically smallest way. If the ball cannot reach the hole, output "impossible".

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The ball and the hole coordinates are represented by row and column indexes.

Example 1:
```
Input 1: a maze represented by a 2D array

0 0 0 0 0
1 1 0 0 1
0 0 0 0 0
0 1 0 0 1
0 1 0 0 0

Input 2: ball coordinate (rowBall, colBall) = (4, 3)
Input 3: hole coordinate (rowHole, colHole) = (0, 1)

Output: "lul"

Explanation: There are two shortest ways for the ball to drop into the hole.
The first way is left -> up -> left, represented by "lul".
The second way is up -> left, represented by 'ul'.
Both ways have shortest distance 6, but the first way is lexicographically smaller because 'l' < 'u'. So the output is "lul".
```
Example 2:
```
Input 1: a maze represented by a 2D array

0 0 0 0 0
1 1 0 0 1
0 0 0 0 0
0 1 0 0 1
0 1 0 0 0

Input 2: ball coordinate (rowBall, colBall) = (4, 3)
Input 3: hole coordinate (rowHole, colHole) = (3, 0)

Output: "impossible"

Explanation: The ball cannot reach the hole.
```
 

Note:

There is only one ball and one hole in the maze.
Both the ball and hole exist on an empty space, and they will not be at the same position initially.
The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
The maze contains at least 2 empty spaces, and the width and the height of the maze won't exceed 30.
```c++
class Solution {
public:
    string findShortestWay(vector<vector<int>>& maze, vector<int>& ball, vector<int>& hole) {
        if (ball[0] == hole[0] && ball[1] == hole[1]) {
            return "";
        }
        const int m = maze.size();
        if (0 == m) {
            return "impossible";
        }
        const int n = maze[0].size();
        if (0 == n) {
            return "impossible";
        }
        
        unordered_map<int, pair<int, string>> visit;
        priority_queue<node, vector<node>, cmp> pq;
        pq.push(node(ball[0], ball[1], 0));
        while (!pq.empty()) {
            node cur = pq.top();
            pq.pop();
            //cout << "[" << cur.x << ", " << cur.y << "]: " << cur.cost << " " << cur.path << endl;
            if (cur.x == hole[0] && cur.y == hole[1]) {
                return cur.path;
            }
            for (int i = 0; i < 4; i++) {
                int x = cur.x;
                int y = cur.y;
                int cost = cur.cost;
                string path = cur.path + dir[i];
                while (x >= 0 && x < m && y >= 0 && y < n && maze[x][y] != 1 && !(x == hole[0] && y == hole[1])) {
                    x += dirs[i].first;
                    y += dirs[i].second;
                    cost++;
                }
                if (!(x == hole[0] && y == hole[1])) {
                    x -= dirs[i].first;
                    y -= dirs[i].second;
                    cost--;
                    if (x == cur.x && y == cur.y) {
                        continue;
                    }
                }
                if (visit.count(x*n+y) == 0 || cost < visit[x*n+y].first || (cost == visit[x*n+y].first && path < visit[x*n+y].second)) {
                    visit[x*n+y].first = cost;
                    visit[x*n+y].second = path;
                    pq.push(node(x, y, cost, path));
                }
            }
        }
        
        return "impossible";
    }
    
private:
    struct node {
        int x;
        int y;
        int cost;
        string path;
        node(int x, int y, int cost, string path = ""): x(x), y(y), cost(cost), path(path) {}
    };
    
    struct cmp {
        bool operator()(const node& a, const node& b) {
            return a.cost > b.cost || (a.cost == b.cost && a.path > b.path);
        }
    };
    
    vector<pair<int, int>> dirs = { {1, 0}, {-1, 0}, {0, 1}, {0, -1} };
    
    vector<char> dir = { 'd', 'u', 'r', 'l' };
};
```