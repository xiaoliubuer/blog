---
layout: post
title:  "661. Image Smoother"
date: 2019-08-18 16:16:00 -0400
categories: articles
---
Given a 2D integer matrix M representing the gray scale of an image, you need to design a smoother to make the gray scale of each cell becomes the average gray scale (rounding down) of all the 8 surrounding cells and itself. If a cell has less than 8 surrounding cells, then use as many as you can.

Example 1:
```
Input:
[[1,1,1],
 [1,0,1],
 [1,1,1]]
Output:
[[0, 0, 0],
 [0, 0, 0],
 [0, 0, 0]]
```
Explanation:
```
For the point (0,0), (0,2), (2,0), (2,2): floor(3/4) = floor(0.75) = 0
For the point (0,1), (1,0), (1,2), (2,1): floor(5/6) = floor(0.83333333) = 0
For the point (1,1): floor(8/9) = floor(0.88888889) = 0
```
Note:
```
The value in the given matrix is in the range of [0, 255].
The length and width of the given matrix are in the range of [1, 150].
```
```c++
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
        int m = M.size(), n = M[0].size();
        if (m == 0 || n == 0) return { { } };
        vector<vector<int>> dirs = { {0,1},{0,-1},{1,0},{-1,0},{-1,-1},{1,1},{-1,1},{1,-1} };
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int sum = M[i][j], cnt = 1;
                for (int k = 0; k < dirs.size(); k++) {
                    int x = i + dirs[k][0], y = j + dirs[k][1];
                    if (x < 0 || x > m - 1 || y < 0 || y > n - 1) continue;
                    sum += (M[x][y] & 0xFF);
                    cnt++;
                }
                M[i][j] |= ((sum / cnt) << 8);
            }
        }
         for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                M[i][j] >>= 8;
            }
         }
        return M;
    }
};
```
```c++
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>> &M) {
        int m = M.size();
        int n = M[0].size();
        vector<vector<int>> result(m, vector<int>(n));
        int range[] = {-1, 0, 1};
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                int current = 0;
                int count = 0;
                for (int a : range) {
                    for (int b : range) {
                        int row = i + a;
                        int col = j + b;
                        if (row >= 0 && row < m && col >= 0 && col < n) {
                            current += M[row][col];
                            count++;
                        }
                    }
                }
                result[i][j] = (int) floor((double) current / count);
            }
        }
        return result;
    }
};
```
```c++
class Solution {
public:
    bool isValidRow(int rowIndex, int maxMatrixRow)
    {
        if(rowIndex >=0 && rowIndex <= maxMatrixRow)
        {
            return true;
        }
        
        return false;    
    }
    
    bool isValidCol(int colIndex, int maxMatrixCol)
    {
        if(colIndex >=0 && colIndex <= maxMatrixCol)
        {
            return true;
        }
        
        return false;    
    }
    
    
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {      
        
        vector<vector<int>> result(M.size(), vector<int>(M[0].size()));
        for(int r = 0; r < M.size(); ++r)
        {
            for(int c = 0; c < M[0].size(); ++c)
            {
                int count = 0;
                int sum = 0;
                for(int i = -1; i < 2; ++i)
                {
                    for(int j = -1; j < 2; ++j)
                    {
                        //cout<< "1. hello" << endl;
                        int maxRowNum = M.size() - 1;
                        int maxColNum = M[0].size() - 1;
                        if(isValidRow(r+i, maxRowNum) && isValidCol(c + j, maxColNum))
                        {
                            sum += M[r+i][c+j];
                            ++count;
                        }
                        
                    }
                }
                
                int average = sum / count;
                result[r][c] = average;
            }
        }
        
        return result;
        
        
    }
};
```

```c++
class Solution {
public:
    vector<vector<int>> imageSmoother(vector<vector<int>>& M) {
        vector< vector<int> > ans = M;
        for(int i = 0; i < M.size(); i++) {
            for(int j = 0; j < M[0].size(); j++) {
                int sum = M[i][j];
                int x[] = {1, 1, 1, -1, -1, -1, 0, 0};
                int y[] = {1, -1, 0, 1, -1, 0, 1, -1};
                int elems = 1;
                for(int k = 0; k < 8; k++) {
                    if(y[k] + i >= 0 && y[k] + i < M.size() && x[k] + j >= 0 && x[k] + j < M[0].size()) {
                        sum += M[y[k] + i][x[k] + j];
                        elems++;
                    }
                }
                ans[i][j] = floor(sum/elems);
            }
        }
        return ans;
    }
};
```