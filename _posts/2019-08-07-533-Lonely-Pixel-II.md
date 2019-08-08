---
layout: post
title:  "533. Lonely Pixel II"
date: 2019-08-07 21:17:00 -0400
categories: articles
---
Given a picture consisting of black and white pixels, and a positive integer N, find the number of black pixels located at some specific row R and column C that align with all the following rules:

Row R and column C both contain exactly N black pixels.
For all rows that have a black pixel at column C, they should be exactly the same as row R
The picture is represented by a 2D char array consisting of 'B' and 'W', which means black and white pixels respectively.

Example:
```
Input:                                            
[['W', 'B', 'W', 'B', 'B', 'W'],    
 ['W', 'B', 'W', 'B', 'B', 'W'],    
 ['W', 'B', 'W', 'B', 'B', 'W'],    
 ['W', 'W', 'B', 'W', 'B', 'W']] 

N = 3
Output: 6
Explanation: All the bold 'B' are the black pixels we need (all 'B's at column 1 and 3).
        0    1    2    3    4    5         column index                                            
0    [['W', 'B', 'W', 'B', 'B', 'W'],    
1     ['W', 'B', 'W', 'B', 'B', 'W'],    
2     ['W', 'B', 'W', 'B', 'B', 'W'],    
3     ['W', 'W', 'B', 'W', 'B', 'W']]    
row index

Take 'B' at row R = 0 and column C = 1 as an example:
Rule 1, row R = 0 and column C = 1 both have exactly N = 3 black pixels. 
Rule 2, the rows have black pixel at column C = 1 are row 0, row 1 and row 2. They are exactly the same as row R = 0.
```
Note:
The range of width and height of the input 2D array is [1,200].

```c++
class Solution {
public:
    int findBlackPixel(vector<vector<char>>& picture, int N) {
        int ans = 0;
        if(picture.size() == 0 || N == 0)
            return ans;
        int n = picture.size(), m = picture[0].size();
        vector<int> rbcnt(n, 0), clcnt(m, 0);
        vector<string> rptn(n, "");
        vector<vector<int>> rclmp(m, vector<int>(0));
        for(int i = 0; i < n; ++ i){
            for(int j = 0; j < m; ++ j){
                if(picture[i][j] == 'B'){
                    ++ rbcnt[i];
                    ++ clcnt[j];
                    rclmp[j].push_back(i);
                }
                rptn[i] += picture[i][j];
            }
        }
        for(int i = 0; i < m; ++ i){
            if(clcnt[i] == N){
                if(check(rbcnt, rptn, rclmp, i, N))
                    ans += N;
            }
        }
        return ans;
    }
private:
    bool check(vector<int> &rbcnt, vector<string> &rptn, vector<vector<int>> &rclmp, int col, int N){
        string ptn = "";
        for(int i = 0; i < rclmp[col].size(); ++ i){
            if(i == 0)
                ptn = rptn[rclmp[col][i]];
            else if(ptn != rptn[rclmp[col][i]])
                return false;
            if(rbcnt[rclmp[col][i]] != N)
                return false;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    int findBlackPixel(vector<vector<char>>& picture, int N) {
        map<string,int > M;
        int r = picture[0].size();
        int col[r]={0};
        
        for(int i=0;i<picture.size();i++){
            string str;
            for(int j=0;j<picture[0].size();j++)
                str+=picture[i][j];
            M[str]++;    
        }
        
        for(int j=0;j<picture[0].size();j++){ 
            for(int i=0;i<picture.size();i++){
                if(picture[i][j]=='B')
                    col[j]++;
            }
        }
        
        int ans=0;
        map<string,int>::iterator it;
        for(it=M.begin();it!=M.end();it++){
            string str = it->first;
           // cout<<str<<"\n";
            int cnt =0,flag=0;
            if(it->second==N){
            for(int i=0;i<str.length();i++){
                if(str[i]=='B')
                    cnt++;
            }
         
                if(cnt==N){
                for(int i=0;i<str.length();i++){
                    cout<<col[i]<<" ";
                   if(str[i]=='B' &&  col[i]==cnt)
                        ans+=N;  
                    }
                 }
            }
        }
        
        return ans;
    }
};
```
```c++
class Solution {
public:
    int findBlackPixel(vector<vector<char>>& board, int N) {
        
        int m = board.size(), n = m == 0 ? 0 : board[0].size();
        vector<int> rows (m);
        vector<int> cols (n);
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(board[i][j] == 'B') {
                    ++rows[i];
                    ++cols[j];
                }
            }
        }
        int count = 0;
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(board[i][j] == 'B' && rows[i] == cols[j] && rows[i] == N) {
                    bool flag = true;
                    for(int k = 0; k < m; k++) {
                        if(board[k][j] == 'B') {
                            if(board[k] != board[i]) {
                                flag = false;
                                break;
                            }
                        }
                    }
                    count += flag;
                }
            }
        }  
        return count;
        
        
    }
};
```