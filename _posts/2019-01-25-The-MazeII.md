---
layout: post
title:  "505. The Maze II"
date: 2019-01-25 20:57:23 -0400
categories: articles
---
There is a ball in a maze with empty spaces and walls. The ball can go through empty spaces by rolling up, down, left or right, but it won't stop rolling until hitting a wall. When the ball stops, it could choose the next direction.

Given the ball's start position, the destination and the maze, find the shortest distance for the ball to stop at the destination. The distance is defined by the number of empty spaces traveled by the ball from the start position (excluded) to the destination (included). If the ball cannot stop at the destination, return -1.

The maze is represented by a binary 2D array. 1 means the wall and 0 means the empty space. You may assume that the borders of the maze are all walls. The start and destination coordinates are represented by row and column indexes.

 

Example 1:
```
Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (4, 4)

Output: 12

Explanation: One shortest way is : left -> down -> left -> down -> right -> down -> right.
             The total distance is 1 + 1 + 3 + 1 + 2 + 2 + 2 = 12.
```
Example 2:
```
Input 1: a maze represented by a 2D array

0 0 1 0 0
0 0 0 0 0
0 0 0 1 0
1 1 0 1 1
0 0 0 0 0

Input 2: start coordinate (rowStart, colStart) = (0, 4)
Input 3: destination coordinate (rowDest, colDest) = (3, 2)

Output: -1

Explanation: There is no way for the ball to stop at the destination.
```
 

Note:
```
There is only one ball and one destination in the maze.
Both the ball and the destination exist on an empty space, and they will not be at the same position initially.
The given maze does not contain border (like the red rectangle in the example pictures), but you could assume the border of the maze are all walls.
The maze contains at least 2 empty spaces, and both the width and height of the maze won't exceed 100.
```
# Function signature
```c++
class Solution {
public:
    int shortestDistance(vector<vector<int>>& maze, vector<int>& start, vector<int>& destination) {
        
    }
};
```
# 题意
卧槽，这次是要找出最短的路线。
# 想法
这尼玛，找着就很不错啦，还tm要找到最短的。那就是把找到的那个tm存起来呗。然后最后比较那个最短。
# 尝试解解
```c++
class Solution {
public:
	int res = 100;
	vector<vector<int>> dirt = { {1, 0}, {-1, 0}, {0, 1}, {0, -1} };

    int shortestDistance(vector<vector<int>>& maze, vector<int>& start, vector<int>& destination) {
    	int m = maze.size(), n = maze[0].size();
    	if ( m == 0 || n == 0 ) return 0;
    	vector<vector<int>> visited(m, vector<int>(n, 0));
    	visited[start[0]][start[1]] = 1;
    	helper(maze, start, destination, visited, 0);
    	return res;
    }

    void helper(vector<vector<int>>& maze, vector<int> curr, vector<int>& dest, vector<vector<int>>& visited, int steps){
    	if (curr == dest) {
    		res = min(res, steps);
    		return;
    	}
    	int m = maze.size(), n = maze[0].size();
    	for(auto i : dirt){
    		int x = curr[0], y = curr[1];
    		while ( x >= 0 && x < m && y >= 0 && y < n && !maze[x][y]){
    			steps++;
    			x += i[0];
    			y += i[1];
    		}
    		steps--;
    		x -= i[0];
    		y -= i[1];
    		if ( visited[x][y] == 0 ){
    			visited[x][y] = 1;
    			helper( maze, {x, y}, dest, visited, steps + 1 );
    		}
    	}
    }
};
```
```java
class Solution {
    private int[][] directions = { {-1, 0}, {1, 0}, {0, -1}, {0, 1} };
    
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        int rows = maze.length, cols = maze[0].length;
        int s = start[0] * cols + start[1];
        int target = destination[0] * cols + destination[1];
        Queue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);
        boolean[] visited = new boolean[rows * cols];
        // length 2 array: first element -> index (1D), 2nd element -> path cost 
        pq.offer(new int[]{s, 0});
        // visited[s] = true;
        while (!pq.isEmpty()) {
            // Dijkstra algorithm: the time we pop some element from the priroty queue 
            // we are guaranteed to find the minimum path cost to it 
            int[] curr = pq.poll();
            visited[curr[0]] = true;
            if (curr[0] == target) return curr[1];
            
            int currIdx = curr[0], currCost = curr[1];
            int r = currIdx / cols, c =  currIdx % cols;
            for (int[] direction : directions) {
                int rr = r, cc = c, num = 0;
                while (isValid(rr + direction[0], cc + direction[1], rows, cols, maze)) {
                    rr += direction[0];
                    cc += direction[1];
                    num += 1;
                }
                if (num == 0) continue;
                int next = rr * cols + cc;
                if (visited[next]) continue;
                pq.offer(new int[]{next, currCost + num});
            }
        }
        return -1;
    }
    
    private boolean isValid(int r, int c, int rows, int cols, int[][] maze) {
        return r >= 0 && r < rows && c >= 0 && c < cols && maze[r][c] == 0;
    }
}
```