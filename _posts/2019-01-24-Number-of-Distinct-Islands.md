---
layout: post
title:  "694. Number of Distinct Islands"
date: 2019-01-24 18:56:23 -0400
categories: articles
---
Given a non-empty 2D array grid of 0's and 1's, an island is a group of 1's (representing land) connected 4-directionally (horizontal or vertical.) You may assume all four edges of the grid are surrounded by water.

Count the number of distinct islands. An island is considered to be the same as another if and only if one island can be translated (and not rotated or reflected) to equal the other.

Example 1:
```
11000
11000
00011
00011
```
Given the above grid map, return 1.
Example 2:
```
11011
10000
00001
11011
```
Given the above grid map, return 3.

Notice that:
```
11
1
```
and
```
 1
11
```
are considered different island shapes, because we do not consider reflection / rotation.
Note: The length of each dimension in the given grid does not exceed 50.
# Function signature
```c++
class Solution {
public:
    int numDistinctIslands(vector<vector<int>>& grid) {
        
    }
};
```
# 题意
有多少个岛屿。1是陆地，0是水。只要上下左右不连着就是一个岛。
# 想法
对每个点进行DFS。完了之后个 res++。 扫过的岛做标记。
__如何判断这个岛是distinct的呢？__
这个就很有意思啦！！
对呀，就把那个点当前走的方向放进到一个string里面。然后对比这个stirng是不是有重复。

```
注意！！
这样4个方向的DFS，有个问题，就是判断要不要进入下一个递归，是要在前一层进行判断呢？？还是要在进去之后验证这一步对不对？？
怎样更好一点呢？？

如果下一层能走，我才会走，如果不能走，我就不走了。这样的话我就得在上一层对下一层的情况进行各种各样的处理。

如果是进入到下一层再说呢？？在上一层先不管三七二十一，先走进去再说。不行就直接退出来。
```
# 尝试解解
```c++
// 完全错误！！！ 终于Accepted！！了
class Solution {
public:
	void helper(vector<vector<int>>& grid, int i, int j, string& path, string dir){
		int m = grid.size(), n = grid[0].size();
		if ( i < 0 || i >= m || j < 0 || j >= n || grid[i][j] <= 0) return;
		path += dir;
		grid[i][j] = -1;
		helper(grid, i - 1, j, path, "d");
		helper(grid, i, j + 1, path, "r");
        helper(grid, i + 1, j, path, "u");
		helper(grid, i, j - 1, path, "l");
        path += "b"; // 这里太重要啦！！！对于DFS还是理解不够深刻啊！！
	}

    int numDistinctIslands(vector<vector<int>>& grid) {
    	if ( grid.size() == 0 || grid[0].size() == 0 ) return 0;
    	unordered_set<string> exist;
    	for (int i = 0; i < grid.size(); ++i){
    		for (int j = 0; j < grid[0].size(); ++j){
    			if ( grid[i][j] > 0 ){
    				string path = "";
    				helper(grid, i, j, path, "o");
    				exist.insert(path);
    			}
    		}
    	}
        return exist.size();
    }
};
```

```c++
// Accepted!!!
class Solution {
public:
    void dfs(vector<vector<int>>& grid, int x, int y, string dir, string& p){
        if ( x < 0 || x >= grid.size() || y < 0 || y >= grid[0].size() || grid[x][y] == 0 ) return;
            p += dir;
            grid[x][y] = 0;
            dfs(grid, x - 1, y, "d", p);
            dfs(grid, x + 1, y, "u", p);
            dfs(grid, x, y - 1, "l", p);
            dfs(grid, x, y + 1, "r", p);
            p += "b"; // 很重要!!! 用来区别每一层的顺序，否则有可能层与层之间形成的顺序是巧合一样的。
    }
    int numDistinctIslands(vector<vector<int>>& grid) {
        unordered_set<string> pattern;
        if (grid.size() == 0 || grid[0].size() == 0) return 0;
        for (int i = 0; i < grid.size(); i++){
            for (int j = 0; j < grid[0].size(); j++){
                if (grid[i][j] == 1) {
                    string p = "";
                    dfs(grid, i, j, "", p); //  这里是“” 还是“o” 无所谓 
                    pattern.insert(p); 
                }
            }
        }
        return pattern.size();
    }
};
```