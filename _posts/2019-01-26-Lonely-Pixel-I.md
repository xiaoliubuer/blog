---
layout: post
title:  "531. Lonely Pixel I"
date: 2019-01-26 15:21:23 -0400
categories: articles
---
Given a picture consisting of black and white pixels, find the number of black lonely pixels.

The picture is represented by a 2D char array consisting of 'B' and 'W', which means black and white pixels respectively.

A black lonely pixel is character 'B' that located at a specific position where the same row and same column don't have any other black pixels.

Example:
```
Input: 
[['W', 'W', 'B'],
 ['W', 'B', 'W'],
 ['B', 'W', 'W']]

Output: 3
```
Explanation: All the three 'B's are black lonely pixels.
Note:
The range of width and height of the input 2D array is [1,500].
# Function signature
```c++
class Solution {
public:
    vector<vector<char>> updateBoard(vector<vector<char>>& board, vector<int>& click) {
        
    }
};
```
# 题意
给一个矩阵，在同一行和列没有别的B。就是longly Black。问这个矩阵里有几个这样的B。
# 想法
建两个数组，一个用来存行的情况，一个用来存列的情况。然后扫描所有的点。如果这个是B，那么看它行和列的数组的这个位置是不是都是0，如果都是，结果就++，如果已经有一个已经有值了，那个结果就渐渐。
# 尝试解解
```c++
// Accepted!! 
class Solution {
public:
    int findLonelyPixel(vector<vector<char>>& picture) {
    	int res = 0;
    	int m = picture.size(), n = picture[0].size();
    	if (m == 0 || n == 0) return res;
    	vector<int> countRow(m, 0);
    	vector<int> countCol(n, 0);

    	for (int i = 0; i < m; ++i){
    		for (int j = 0; j < n; ++j){
    			if (picture[i][j] == 'B'){
    				if (countRow[i] == 0 && countCol[j] == 0)
    					res++;
    				else if( countRow[i] == 1 || countCol[j] == 1)
    					res--;
    				countRow[i]++;
    				countCol[j]++;
    			}
    		}
    	}
        return res < 0 ? 0 : res;
    }
};
```