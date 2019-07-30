---
layout: post
title:  "289. Game of Life"
date: 2019-07-28 20:00:00 -0400
categories: articles
---
According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."

Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):

Any live cell with fewer than two live neighbors dies, as if caused by under-population.
Any live cell with two or three live neighbors lives on to the next generation.
Any live cell with more than three live neighbors dies, as if by over-population..
Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
Write a function to compute the next state (after one update) of the board given its current state. The next state is created by applying the above rules simultaneously to every cell in the current state, where births and deaths occur simultaneously.

Example:
```
Input: 
[
  [0,1,0],
  [0,0,1],
  [1,1,1],
  [0,0,0]
]
Output: 
[
  [0,0,0],
  [1,0,1],
  [0,1,1],
  [0,1,0]
]
```
Follow up:
```
Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?
```
```c++
// Game of Life
/*
状态: 前一位表示下一代的状态,后一位表示当前的状态
00: 死->死
10: 死->活
01: 活->死
11: 活->活
*/
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        int d[][2] = { { 1,-1 } ,{ 1, 0 },{ 1, 1 },{ 0, -1 }, { 0, 1 }, { -1, -1 }, { -1, 0 },{ -1, 1 } };
        for(int i = 0; i < board.size(); i++){
            for(int j = 0; j < board[0].size(); j++){
                int live = 0;
                for(int k = 0; k < 8; k++){
                    int x = d[k][0] + i;
                    int y = d[k][1] + j;
                    if(x < 0 || x >= board.size() || y < 0 || y >= board[0].size()) {
                        continue;
                    }
                    if(board[x][y] & 1) {
                        live++;
                    }
                }
                // 死的
                if(board[i][j] == 0) {
                    if(live == 3){
                        board[i][j] = 2; // 2 : (10)
                    }
                }
                // 活的
                else {
                    if(live < 2 || live > 3){
                        board[i][j] = 1; // 1 : (01)
                    }else{
                        board[i][j] = 3; // 3 : (11)   
                    }
                }
            }
        }
        for(int i = 0; i < board.size(); i++){
            for(int j=0; j < board[0].size(); j++){
                board[i][j] >>=1;
            }
        }
    }
};
```

```c++
class Solution {
public:
void gameOfLife(vector<vector<int>>& board) {
    int m = board.size(), n = m ? board[0].size() : 0;
    for (int i=0; i<m; ++i) {
        for (int j=0; j<n; ++j) {
            int count = 0;
            for (int I=max(i-1, 0); I<min(i+2, m); ++I)
                for (int J=max(j-1, 0); J<min(j+2, n); ++J)
                    count += board[I][J] & 1;
            if (count == 3 || count - board[i][j] == 3)
                board[i][j] |= 2;
        }
    }
    for (int i=0; i<m; ++i)
        for (int j=0; j<n; ++j)
            board[i][j] >>= 1;
}
};
```