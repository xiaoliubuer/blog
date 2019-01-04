---
layout: post
title:  "348. Design Tic-Tac-Toe"
date: 2018-12-31 17:16:23 -0400
categories: articles
---
Design a Tic-tac-toe game that is played between two players on a n x n grid.

You may assume the following rules:

A move is guaranteed to be valid and is placed on an empty block.
Once a winning condition is reached, no more moves is allowed.
A player who succeeds in placing n of their marks in a horizontal, vertical, or diagonal row wins the game.
Example:
```
Given n = 3, assume that player 1 is "X" and player 2 is "O" in the board.

TicTacToe toe = new TicTacToe(3);

toe.move(0, 0, 1); -> Returns 0 (no one wins)
|X| | |
| | | |    // Player 1 makes a move at (0, 0).
| | | |

toe.move(0, 2, 2); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 2 makes a move at (0, 2).
| | | |

toe.move(2, 2, 1); -> Returns 0 (no one wins)
|X| |O|
| | | |    // Player 1 makes a move at (2, 2).
| | |X|

toe.move(1, 1, 2); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 2 makes a move at (1, 1).
| | |X|

toe.move(2, 0, 1); -> Returns 0 (no one wins)
|X| |O|
| |O| |    // Player 1 makes a move at (2, 0).
|X| |X|

toe.move(1, 0, 2); -> Returns 0 (no one wins)
|X| |O|
|O|O| |    // Player 2 makes a move at (1, 0).
|X| |X|

toe.move(2, 1, 1); -> Returns 1 (player 1 wins)
|X| |O|
|O|O| |    // Player 1 makes a move at (2, 1).
|X|X|X|
```
Follow up:
Could you do better than O(n2) per move() operation?

# Function signature
```c++
class TicTacToe {
public:
    /** Initialize your data structure here. */
    TicTacToe(int n) {
        
    }
    
    /** Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins. */
    int move(int row, int col, int player) {
        
    }
};

/**
 * Your TicTacToe object will be instantiated and called as such:
 * TicTacToe obj = new TicTacToe(n);
 * int param_1 = obj.move(row,col,player);
 */
```

# 题意
设计一个游戏，让两个玩家依次下棋，只要有一个玩家获胜，游戏结束。获胜条件是只要横，竖，斜有3子连成一条线即可。

# 想法
如何实现判断获胜？
row[n] = {};
col[n] = {};
因为对角线只有一条，所以就只用一个数字来表示他现在有的个数。
int dig = 0;
int antidig = 0;

1. Init containers
```c++
vector<int> rows;
vector<int> cols;
int dig;
int antidig;
```
2. Constructor
```c++
    TicTacToe(int n):rows(n, 0), cols(n,0), dig(0), antidig(0) {
    }
```
3. Implement move:
如何标记是哪个player在玩是个很有意思的问题。但是你发现，只要这一行，或一列，或一斜，只要有一个被对手污染了，那么这里就永远无法成功了。所以我们可以用1和-1来标记，随后来算绝对值，如果绝对值等于n，那么就胜利了!
```c++
    int move(int row, int col, int player) {
    	int steps = player == 1 ? 1 : -1;
    	int n = row.size();
    	rows[row] += steps;
    	cols[col] += steps;

    	if ( row == col) dig += steps;
    	if ( row == n - col - 1) antidig += steps;
    	if ( abs(rows[row]) == n || abs(cols[col]) == n || dig == n || antidig == n) return player;
    	return 0;
    }
```

# 参考答案
```c++
class TicTacToe {
public:
    /** Initialize your data structure here. */
    TicTacToe(int n): rows(n , 0), cols(n, 0), dig(0), antidig(0) {
    }
    int move(int row, int col, int player) {
        int steps = player == 1 ? 1 : -1;
        int n = rows.size();
        rows[row] += steps;
        cols[col] += steps;
        if ( row == col ) dig += steps;
        if ( row == n - col - 1) antidig += steps;
        if ( abs(rows[row]) == n || abs(cols[col]) == n || abs(dig) == n || abs(antidig) == n)
            return player;
        return 0;
    }
private:
    vector<int> rows;
    vector<int> cols;
    int dig;
    int antidig;
};
```