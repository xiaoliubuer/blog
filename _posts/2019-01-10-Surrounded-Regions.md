---
layout: post
title:  ". 130. Surrounded Regions"
date: 2019-01-10 20:48:23 -0400
categories: articles
---
Given a 2D board containing 'X' and 'O' (the letter O), capture all regions surrounded by 'X'.

A region is captured by flipping all 'O's into 'X's in that surrounded region.

Example:
```
X X X X
X O O X
X X O X
X O X X
```
After running your function, the board should be:
```
X X X X
X X X X
X X X X
X O X X
```
Explanation:

Surrounded regions shouldn’t be on the border, which means that any 'O' on the border of the board are not flipped to 'X'. Any 'O' that is not on the border and it is not connected to an 'O' on the border will be flipped to 'X'. Two cells are connected if they are adjacent cells connected horizontally or vertically.
# Function signature
```c++
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        
    }
};
```
# 题意
就是在这个2D board里面，翻转所有被X包围的O。
# 想法
DFS，找到就DFS，发现确定是被包着了，那么久把这一部分的点都翻转了。
# 试着解解
```c++
class Solution {
public:
	bool helper(vector<vector<char>>& board, int x, int y){
		if ( (x == 0 || x == board.size() - 1 || y == 0 || y == board[0].size() -1) && board[x][y] == 'O' ){
			board[x][y] = 'F';
			return false;
		}
			bool d = false, u = false, l = false, r = false; 
			if ( x - 1 >= 0 && board[x-1][y] == 'O' ) d = helper(board, x-1, y);
			if ( x + 1 < board.size() && board[x+1][y] == 'O') u = helper(board, x+1, y);
			if ( y - 1 >= 0 && board[x][y-1] == 'O' ) l = helper(board, x, y-1);
			if ( y + 1 < board[0].size() && board[x][y+1]) r = helper(board, x, y+1);
			if ( d && u && l && r)
				board[x][y] = 'X';
			else
				board[x][y] = 'F';
        return true;
	}

    void solve(vector<vector<char>>& board) {
    	if (board.size() == 0 || board[0].size() == 0) return;
    	for (int i = 0; i < board.size(); ++i){
    		for (int j = 0; j < board[0].size() ; ++j){
                if (board[i][j] == 'O') helper(board, i, j);
    		}
    	}

    	for (int i = 0; i < board.size(); ++i){
    		for (int j = 0; j < board[0].size() ; ++j){
    			if (board[i][j] == 'F')
    				board[i][j] = 'O';
    		}
    	}
    }
};
```
# 参考答案
```c++
public void solve(char[][] board) {
	if (board.length == 0 || board[0].length == 0)
		return;
	if (board.length < 2 || board[0].length < 2)
		return;
	int m = board.length, n = board[0].length;
	//Any 'O' connected to a boundary can't be turned to 'X', so ...
	//Start from first and last column, turn 'O' to '*'.
	for (int i = 0; i < m; i++) {
		if (board[i][0] == 'O')
			boundaryDFS(board, i, 0);
		if (board[i][n-1] == 'O')
			boundaryDFS(board, i, n-1);	
	}
	//Start from first and last row, turn '0' to '*'
	for (int j = 0; j < n; j++) {
		if (board[0][j] == 'O')
			boundaryDFS(board, 0, j);
		if (board[m-1][j] == 'O')
			boundaryDFS(board, m-1, j);	
	}
	//post-prcessing, turn 'O' to 'X', '*' back to 'O', keep 'X' intact.
	for (int i = 0; i < m; i++) {
		for (int j = 0; j < n; j++) {
			if (board[i][j] == 'O')
				board[i][j] = 'X';
			else if (board[i][j] == '*')
				board[i][j] = 'O';
		}
	}
}
//Use DFS algo to turn internal however boundary-connected 'O' to '*';
private void boundaryDFS(char[][] board, int i, int j) {
	if (i < 0 || i > board.length - 1 || j <0 || j > board[0].length - 1)
		return;
	if (board[i][j] == 'O')
		board[i][j] = '*';
	if (i > 1 && board[i-1][j] == 'O')
		boundaryDFS(board, i-1, j);
	if (i < board.length - 2 && board[i+1][j] == 'O')
		boundaryDFS(board, i+1, j);
	if (j > 1 && board[i][j-1] == 'O')
		boundaryDFS(board, i, j-1);
	if (j < board[i].length - 2 && board[i][j+1] == 'O' )
		boundaryDFS(board, i, j+1);
}
```