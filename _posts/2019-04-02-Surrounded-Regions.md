---
layout: post
title:  "130. Surrounded Regions"
date: 2019-04-02 22:32:23 -0400
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

```c++
class Solution {
public:
    void solve(vector<vector<char>>& board) {
        if (board.empty()) return;
        int row = board.size(), col = board[0].size();
        for (int i = 0; i < row; ++i) {
            check(board, i, 0);             // first column
            check(board, i, col - 1);       // last column
        }
        for (int j = 1; j < col - 1; ++j) {
            check(board, 0, j);             // first row
            check(board, row - 1, j);       // last row
        }
        for (int i = 0; i < row; ++i)
            for (int j = 0; j < col; ++j)
                if (board[i][j] == 'O') board[i][j] = 'X';
                else if (board[i][j] == '1') board[i][j] = 'O';
    }
    
    void check(vector<vector<char>>& board, int i, int j) {
        if (board[i][j] == 'O') {
            board[i][j] = '1';
            if (i > 1) check(board, i - 1, j);
            if (j > 1) check(board, i, j - 1);
            if (i + 1 < board.size()) check(board, i + 1, j);
            if (j + 1 < board[0].size()) check(board, i, j + 1);
        }
    }
};
```