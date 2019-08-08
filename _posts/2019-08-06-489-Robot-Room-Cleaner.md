---
layout: post
title:  "489. Robot Room Cleaner"
date: 2019-08-06 20:20:00 -0400
categories: articles
---
Given a robot cleaner in a room modeled as a grid.

Each cell in the grid can be empty or blocked.

The robot cleaner with 4 given APIs can move forward, turn left or turn right. Each turn it made is 90 degrees.

When it tries to move into a blocked cell, its bumper sensor detects the obstacle and it stays on the current cell.

Design an algorithm to clean the entire room using only the 4 given APIs shown below.

interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
Example:
```
Input:
room = [
  [1,1,1,1,1,0,1,1],
  [1,1,1,1,1,0,1,1],
  [1,0,1,1,1,1,1,1],
  [0,0,0,1,0,0,0,0],
  [1,1,1,1,1,1,1,1]
],
row = 1,
col = 3
```
Explanation:
```
All grids in the room are marked by either 0 or 1.
0 means the cell is blocked, while 1 means the cell is accessible.
The robot initially starts at the position of row=1, col=3.
From the top left corner, its position is one row below and three columns right.
```
Notes:
```
The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the mentioned 4 APIs, without knowing the room layout and the initial robot's position.
The robot's initial position will always be in an accessible cell.
The initial direction of the robot will be facing up.
All accessible cells are connected, which means the all cells marked as 1 will be accessible by the robot.
Assume all four edges of the grid are all surrounded by wall.
```

```c++
class Solution {
public:
    vector<int> dx = { -1, 0, 1, 0 };
    vector<int> dy = { 0, 1, 0, -1 };
    int curDir = 0;
    unordered_map<int, unordered_map<int, int>> visited;
    int x = 0;
    int y = 0;
    void cleanRoom(Robot& robot) {
        //direction 0: face up; 1: face right, 2: face down, 3: face left
        if(visited[x][y] == 1)
            return;
        visited[x][y] = 1;
        robot.clean();
        for(int i = 0; i < 4; i++){
            if(robot.move()){
                x += dx[curDir];
                y += dy[curDir];
                cleanRoom(robot);
                robot.turnLeft();
                robot.turnLeft();
                robot.move();
                robot.turnLeft();
                robot.turnLeft();
                x -= dx[curDir];
                y -= dy[curDir];
            }
            robot.turnRight();
            curDir = (curDir + 1) % 4;
        }
    }
    
};
```

```c++
class Solution {
public:
    void cleanRoom(Robot& robot) {
        if(visited.count( { x , y } ) ) return;
        visited.insert( { x , y } );
        robot.clean();
        for(int i=0;i<4;i++)
        {
            if(robot.move())
            {
                // change position
                x+=dir[cur_dir].first;
                y+=dir[cur_dir].second;
                // DFS
                cleanRoom(robot); 
                //move to opposite direct 
                robot.turnRight();
                robot.turnRight();
                robot.move();
                // Return to origianl dirction
                robot.turnRight();
                robot.turnRight();
                // Return to original position
                x-=dir[cur_dir].first;
                y-=dir[cur_dir].second;
            }
            // Currently in original position change to next direction
            robot.turnRight();
            cur_dir=(cur_dir+1)%4;
        }
    }
    
private:
    set<pair<int,int>> visited;
    int x = 0;
    int y = 0;
    vector<pair<int,int>> dir{ { 0, 1 },{ 1, 0 }, { 0, -1 }, { -1, 0 } };
    int cur_dir = 0; 
};
```
```c++
class Solution {
    map<pair<int, int>, int> visited;
    map<pair<int, int>, int> parrent;
    typedef bool func_t(Robot &);
    vector<func_t*> forward = {move_left, move_down, move_right, move_up};
    vector<func_t*> backward = {move_right, move_up, move_left, move_down};
    int pos[4][2] = { { 1,0}, {0, 1}, { -1, 0}, { 0 , -1} };

public:
    void cleanRoom(Robot& robot) {
        visited[ make_pair(0, 0) ] = 1;
        dfs(robot, 0, 0);
    }
    
    void dfs(Robot & robot, int x, int y) {
        robot.clean();
        for(int i=0;i<4; i++) {
            int new_x = x + pos[i][0];
            int new_y = y + pos[i][1];
            if(visited.count(make_pair(new_x, new_y))==0) {
                visited[make_pair(new_x, new_y)]=1;
                if(forward[i](robot)) {
                    parrent[make_pair(new_x, new_y)] = i;
                    dfs(robot, new_x, new_y);
                }
            }
            
        }
        backward[parrent[make_pair(x, y)]](robot);
    }
    
    static bool move_left(Robot & robot) {
        robot.turnLeft();
        bool success = robot.move();
        robot.turnRight();
        return success;
    }
    
    static bool move_right(Robot & robot) {
        robot.turnRight();
        bool success = robot.move();
        robot.turnLeft();
        return success;
    }
    
    static bool move_down(Robot & robot) {
        robot.turnLeft();
        robot.turnLeft();
        bool success = robot.move();
        robot.turnRight();
        robot.turnRight();
        return success;
    }
    
    static bool move_up(Robot & robot) {
        bool success = robot.move();
        return success;
    }
};
```
