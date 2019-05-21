---
layout: post
title:  "913. Cat and Mouse"
date: 2019-05-09 17:37:00 -0400
categories: articles
---
A game on an undirected graph is played by two players, Mouse and Cat, who alternate turns.

The graph is given as follows: graph[a] is a list of all nodes b such that ab is an edge of the graph.

Mouse starts at node 1 and goes first, Cat starts at node 2 and goes second, and there is a Hole at node 0.

During each player's turn, they must travel along one edge of the graph that meets where they are.  For example, if the Mouse is at node 1, it must travel to any node in graph[1].

Additionally, it is not allowed for the Cat to travel to the Hole (node 0.)

Then, the game can end in 3 ways:

If ever the Cat occupies the same node as the Mouse, the Cat wins.
If ever the Mouse reaches the Hole, the Mouse wins.
If ever a position is repeated (ie. the players are in the same position as a previous turn, and it is the same player's turn to move), the game is a draw.
Given a graph, and assuming both players play optimally, return 1 if the game is won by Mouse, 2 if the game is won by Cat, and 0 if the game is a draw.

 

Example 1:

Input: [[2,5],[3],[0,4,5],[1,4,5],[2,3],[0,2,3]]
Output: 0
Explanation:
4---3---1
|   |
2---5
 \ /
  0
 

Note:

3 <= graph.length <= 50
It is guaranteed that graph[1] is non-empty.
It is guaranteed that graph[2] contains a non-zero element. 




```c++
class Solution {
public:
    
    int calc(int curM, int curC, int isM, vector<vector<vector<int>>>& dp, vector<vector<int>>& graph) {
        if(curC==0 || curM==0)
            return 1;
        if(curC==curM)
            return 2;
        if(dp[curM][curC][isM]!=-1) {
            return dp[curM][curC][isM];
        }
        dp[curM][curC][isM]=0;
        bool canDraw=false;
        if(isM) {
            for(int i=0;i<graph[curM].size();i++) {     //If any neigbor is 0 return 1
                if(graph[curM][i]==0) {
                    dp[curM][curC][isM]=1;
                    return 1;
                }
            }
            for(int i=0;i<graph[curM].size();i++) {
                int temp=calc(graph[curM][i], curC, 0, dp, graph);
                if(temp==1) {
                    dp[curM][curC][isM]=1;
                    return 1;
                }
                else if(temp==0) {
                    canDraw=true;
                }
            }
            if(!canDraw)
                dp[curM][curC][isM]=2;
        }
        else {
            for(int i=0;i<graph[curC].size();i++) {     //If any neighbor is current pos of mouse return 2
                if(graph[curC][i]==curM) {
                    dp[curM][curC][isM]=2;
                    return 2;
                }
            }
            for(int i=0;i<graph[curC].size();i++) {
                int temp=calc(curM, graph[curC][i], 1, dp, graph);
                if(temp==2) {
                    dp[curM][curC][isM]=2;
                    return 2;
                }
                else if(temp==0) {
                    canDraw=true;
                }
            }
            if(!canDraw)
                dp[curM][curC][isM]=1;
        }
        return dp[curM][curC][isM];
    }
    
    int catMouseGame(vector<vector<int>>& graph) {
        int n=graph.size();
        vector<vector<vector<int>>> dp(n, vector<vector<int>>(n, vector<int>(2, -1)));
        return calc(1, 2, 1, dp, graph);
    }
};
```