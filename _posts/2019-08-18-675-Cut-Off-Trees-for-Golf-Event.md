---
layout: post
title:  "675. Cut Off Trees for Golf Event"
date: 2019-08-18 16:57:00 -0400
categories: articles
---
You are asked to cut off trees in a forest for a golf event. The forest is represented as a non-negative 2D map, in this map:

0 represents the obstacle can't be reached.
1 represents the ground can be walked through.
The place with number bigger than 1 represents a tree can be walked through, and this positive number represents the tree's height.
 

You are asked to cut off all the trees in this forest in the order of tree's height - always cut off the tree with lowest height first. And after cutting, the original place has the tree will become a grass (value 1).

You will start from the point (0, 0) and you should output the minimum steps you need to walk to cut off all the trees. If you can't cut off all the trees, output -1 in that situation.

You are guaranteed that no two trees have the same height and there is at least one tree needs to be cut off.

Example 1:
```
Input: 
[
 [1,2,3],
 [0,0,4],
 [7,6,5]
]
Output: 6
```
Example 2:
```
Input: 
[
 [1,2,3],
 [0,0,0],
 [7,6,5]
]
Output: -1
```
Example 3:
```
Input: 
[
 [2,3,4],
 [0,0,5],
 [8,7,6]
]
Output: 6
Explanation: You started from the point (0,0) and you can cut off the tree in (0,0) directly without walking.
```
Hint: size of the given matrix will not exceed 50x50.
```c++
class Solution {
public:

	vector<vector<int>> dir = { {0, 1}, {0, -1},{1, 0},{-1, 0} };

	int helper(vector<vector<int>>& forest, int sx, int sy, int dx, int dy){
	if (sx == dx && sy == dy) return 0;
	int m = forest.size();
	int n = forest[0].size();
	queue<pair<int, int>> my_queue;
	vector<vector<int>> visited(m, vector<int>(n ,0));
	visited[sx][sy] = 1;
	int step = 0;
	my_queue.push({sx, sy});
	while(!my_queue.empty()){
		step++;
		int size = my_queue.size();
		for (int i = 0; i < size; ++i){
			int row = my_queue.front().first;
			int col = my_queue.front().second;
			my_queue.pop();

			for (int i = 0; i < 4; ++i){
				int r = row + dir[i][0];
				int c = col + dir[i][1];
				if (r < 0 || r >=m || c < 0 || c >= n || visited[r][c] == 1 || forest[r][c] == 0)
					continue;
				if (r == dx && c == dy) return step;
				visited[r][c] = 1;
				my_queue.push({r, c});
				}
			}
		}
        return -1;
	}

    int cutOffTree(vector<vector<int>>& forest) {
    	if (forest.empty() || forest[0].empty()) return -1;

    	int m = forest.size();
    	int n = forest[0].size();


    	vector<vector<int>> trees;
    	for(int i = 0; i < m; i++){
    		for(int j = 0; j < n; j++){
    			if (forest[i][j] > 1)
    				trees.push_back({forest[i][j], i, j});
    		}
    	}

    	sort(trees.begin(), trees.end());
    	int ans = 0;
    	for(int i = 0, cur_row = 0, cur_col = 0; i < trees.size(); i++){
    		int step = helper(forest, cur_row, cur_col, trees[i][1], trees[i][2]); 
            if (step  == -1) return -1;
    		ans += step;
    		cur_row = trees[i][1];
    		cur_col = trees[i][2];
    	}
    	return ans;
    }
};
```
```c++
class Solution {
public:
    int cutOffTree(vector<vector<int>>& forest) {
        int m = forest.size(), n = forest[0].size(), res = 0, row = 0, col = 0;
        vector<vector<int>> trees;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (forest[i][j] > 1) trees.push_back({forest[i][j], i, j});
            }
        }
        sort(trees.begin(), trees.end());
        for (int i = 0; i < trees.size(); ++i) {
            int cnt = helper(forest, row, col, trees[i][1], trees[i][2]);
            if (cnt == -1) return -1;
            res += cnt;
            row = trees[i][1];
            col = trees[i][2];
        }
        return res;
    }
    int helper(vector<vector<int>>& forest, int row, int col, int treeRow, int treeCol) {
        if (row == treeRow && col == treeCol) return 0;
        int m = forest.size(), n = forest[0].size(), cnt = 0;
        queue<int> q{ { row * n + col } };
        vector<vector<int>> visited(m, vector<int>(n));
        vector<int> dir{-1, 0, 1, 0, -1};
        while (!q.empty()) {
            ++cnt;
            for (int i = q.size(); i > 0; --i) {
                int r = q.front() / n, c = q.front() % n; q.pop();
                for (int k = 0; k < 4; ++k) {
                    int x = r + dir[k], y = c + dir[k + 1];
                    if (x < 0 || x >= m || y < 0 || y >= n || visited[x][y] == 1 || forest[x][y] == 0) continue;
                    if (x == treeRow && y == treeCol) return cnt;
                    visited[x][y] = 1;
                    q.push(x * n + y);
                }
            }
        }
        return -1;
    }
};
```
```c++
class Solution {
    
    int hadlock(int row0, int col0, int destRow, int destCol, vector<vector<int>> &forest) {

        if( row0 == destRow && col0 == destCol)
            return 0;

        int rows = forest.size(), cols = forest[0].size();
        vector<vector<int>> costs( rows, vector<int>(cols, INT_MAX));
        deque<vector<int>> deque;
        int directions[] = { 0, 1, 0, -1, 0 };

        deque.push_back( {row0, col0, 0} );
        costs[row0][col0] = 0;

        while(deque.size() > 0)
        {
            auto cur = deque.front();
            deque.pop_front();
            int r = cur[0], c = cur[1], d = cur[2];
            if( r == destRow && c == destCol)
                return d;
            
            if(costs[r][c] > d)
                continue; 
            
            for (int idx=0; idx < 4; idx++)
            {
                int row = r + directions[idx];
                int col = c + directions[idx+1];
                if (0 <= row && row < rows && 0 <= col && col < cols && costs[row][col] > d+1 && forest[row][col] >= 1)
                {
                    bool closer = abs(row-destRow) < abs(r-destRow)  || abs(col-destCol) < abs(c-destCol);
                    if( closer)
                        deque.push_front( {row, col, d+1});
                    else
                        deque.push_back( {row, col, d+1});
                    
                    costs[row][col] = d+1;
                }
            }
        }

        return -1;
    }
    
public:
    int cutOffTree(vector<vector<int>>& forest) {
        if (forest.size() == 0)
            return 0;

        int numRows = forest.size(), numCols = forest[0].size();

        vector<vector<int>> trees; // vector<int> : height, row, col

        for (int row = 0; row < numRows; row++)
            for (int col = 0; col <numCols; col++)
                if (forest[row][col] > 1)
                {
                    trees.push_back( {forest[row][col], row, col} );
                }

        sort( trees.begin(), trees.end() );

        int row=0, col=0, steps, total=0;
        for(auto &tree: trees)
        {
            steps = hadlock( row, col, tree[1], tree[2], forest);
            if( steps < 0)
                return -1;

            total += steps;
            row = tree[1];
            col = tree[2];
        }

        return total;        
    }
};
```