---
layout: post
title:  "419. Battleships in a Board"
date: 2019-08-01 21:09:00 -0400
categories: articles
---
Given an 2D board, count how many battleships are in it. The battleships are represented with 'X's, empty slots are represented with '.'s. You may assume the following rules:
You receive a valid board, made of only battleships or empty slots.
Battleships can only be placed horizontally or vertically. In other words, they can only be made of the shape 1xN (1 row, N columns) or Nx1 (N rows, 1 column), where N can be of any size.
At least one horizontal or vertical cell separates between two battleships - there are no adjacent battleships.
Example:
```
X..X
...X
...X
In the above board there are 2 battleships.
```
Invalid Example:
```
...X
XXXX
...X
```
This is an invalid board that you will not receive - as battleships will always have a cell separating between them.
Follow up:
Could you do it in one-pass, using only O(1) extra memory and without modifying the value of the board?
```c++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int sum = 0;
        int row = board.size();
        int col = board[0].size();
        
        for(int i=0;i<row;++i)
        {
            for(int j=0;j<col;++j)
            {
                bool flag1 = true, flag2 = true;
                if(board[i][j] == 'X')
                {
                    if(i-1>=0 && board[i-1][j] == 'X') { flag1 = false; }
                    if(j-1>=0 && board[i][j-1] == 'X') { flag2 = false; }
                    if(flag1 && flag2) { ++sum; }
                }
            }
        }
        return sum;
    }
};
```
```c++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int ans = 0;
        for (int i = 0; i < board.size(); ++i) {
            for (int j = 0; j < board[0].size(); ++j) {
                if (i != 0 && board[i - 1][j] == 'X') continue;
                if (j != 0 && board[i][j - 1] == 'X') continue;
                if (board[i][j] == 'X') ans++;
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int m = board.size();
        if (m == 0)
            return 0;
        
        int n = board[0].size();
        int ans = 0;
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                if (i > 0 && board[i - 1][j] == 'X')
                    continue;
                
                if (j > 0 && board[i][j - 1] == 'X')
                    continue;
                
                if (board[i][j] == 'X')
                    ++ans;
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        int currx=0, curry = 0, count = 0;
        for(; currx < board.size(); ++ currx){
            for(curry = 0; curry < board[0].size(); ++ curry){
                if(board[currx][curry] == '.')
                    continue;
                else if((currx == 0 || board[currx-1][curry] == '.') &&
                        (curry == 0 || board[currx][curry-1] == '.')){
                    count ++;
                }
            }
        }
        return count;
    }
};
```