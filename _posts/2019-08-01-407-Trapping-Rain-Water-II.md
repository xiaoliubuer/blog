---
layout: post
title:  "407. Trapping Rain Water II"
date: 2019-08-01 20:27:00 -0400
categories: articles
---
Given an m x n matrix of positive integers representing the height of each unit cell in a 2D elevation map, compute the volume of water it is able to trap after raining.
Note:
Both m and n are less than 110. The height of each unit cell is greater than 0 and is less than 20,000.
Example:
```
Given the following 3x6 height map:
[
  [1,4,3,1,3,2],
  [3,2,1,3,2,4],
  [2,3,3,2,3,1]
]

Return 4.
```
The above image represents the elevation map [ [ 1,4,3,1,3,2 ],[ 3,2,1,3,2,4 ],[ 2,3,3,2,3,1 ] ] before the rain.

After the rain, water is trapped between the blocks. The total volume of water trapped is 4.
```c++
class Solution {
private:
    struct Cell{
        int x, y, height;
        Cell(int x, int y, int height): x(x), y(y), height(height) {}
        bool operator < (const Cell &other) const{
            return height > other.height;
        }
    };
    const int dx[4] = {0, 0, 1, -1};
    const int dy[4] = {1, -1, 0, 0};
public:
    int trapRainWater(vector<vector<int>>& heightMap) {
        if(heightMap.empty()) return 0;
        int ans = 0, m = heightMap.size(), n = heightMap[0].size();
        priority_queue<Cell> que;
        for(int i = 0; i < m; ++i){
            que.push(Cell(i, 0, heightMap[i][0]));
            que.push(Cell(i, n-1, heightMap[i][n-1]));
            heightMap[i][0] = heightMap[i][n-1] = -1;
        }
        for(int j = 1; j < n - 1; ++j){
            que.push(Cell(0, j, heightMap[0][j]));
            que.push(Cell(m-1, j, heightMap[m-1][j]));
            heightMap[0][j] = heightMap[m-1][j] = -1;
        }
        while(!que.empty()){
            Cell c = que.top(); que.pop();
            for(int k = 0; k < 4; ++k){
                int nx = c.x + dx[k], ny = c.y + dy[k];
                if(nx >= m || nx < 0 || ny >= n || ny < 0 || heightMap[nx][ny] == -1) continue;
                if(heightMap[nx][ny] < c.height){
                    ans += c.height - heightMap[nx][ny];
                    que.push(Cell(nx, ny, c.height));
                }
                else
                    que.push(Cell(nx, ny, heightMap[nx][ny]));
                heightMap[nx][ny] = -1;
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int trapRainWater(vector<vector<int>>& heightMap) {
        if(heightMap.empty()) return 0;
        int res=0,m=heightMap.size(),n=heightMap[0].size(),mx=INT_MIN;
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;//由小输出的队列
        vector<vector<bool>> visited(m,vector<bool>(n,false));
        vector<vector<int>> dir={ { 1, 0 }, { -1, 0 }, { 0, 1 }, { 0, -1 } };
        for(int x=0;x<m;x++)
        {
            for(int y=0;y<n;y++){
                if(x==0||x==m-1||y==0||y==n-1){
                q.push({heightMap[x][y],x*n+y});
                visited[x][y]=true;
                }
            }
        }
        while(!q.empty()){
            auto tmp=q.top();
            q.pop();
            int h=tmp.first,r=tmp.second/n,c=tmp.second%n;
            mx=max(mx,h);
            for(int i=0;i<dir.size();i++)
            {
                r=tmp.second/n+dir[i][0];
                c=tmp.second%n+dir[i][1];
                if(r<=0||r>=m-1||c<=0||c>=n-1||visited[r][c]) continue;
                h=heightMap[r][c];
                if(h<mx) res+=mx-h;
                visited[r][c]=true;
                q.push({h,r*n+c});
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
	int trapRainWater(vector<vector<int>>& heightMap) {

		int n=heightMap.size();
		if(n==0)
			return 0;
		int m=heightMap[0].size();
	priority_queue<tuple<int, int, int>,vector<tuple<int,int,int>>,greater<tuple<int,int,int>>>res;
		vector<vector<int>>val(n,vector<int>(m,0));
		for(int i=0;i<n;i++)
		{
			for(int j=0;j<m;j++)
			{
				if(i==0 or j==0 or i==n-1 or j==m-1)
				{
					 res.push(make_tuple(heightMap[i][j], i, j)); 
						val[i][j]=1;
				}
				 }
		}
		int sum=0;
		int maxx=INT_MIN;
		while(!res.empty())
		{
			auto x=res.top();
			res.pop();
			int vall=get<0>(x);
			int i=get<1>(x);
			int j=get<2>(x);
			if(maxx<vall)
				maxx=vall;
			//cout<<maxx<<endl;
			if(i-1>0 and val[i-1][j]!=1)
			{
				if(heightMap[i-1][j]<maxx)
					sum+=maxx-heightMap[i-1][j];
				 res.push(make_tuple(heightMap[i-1][j], i-1, j));
				val[i-1][j]=1;
			}
			if(j-1>0 and val[i][j-1]!=1)
			{
				if(heightMap[i][j-1]<maxx)
					sum+=maxx-heightMap[i][j-1];
				 res.push(make_tuple(heightMap[i][j-1], i, j-1));
				val[i][j-1]=1;
			}
			if(i+1<n and val[i+1][j]!=1)
			{
				if(heightMap[i+1][j]<maxx)
					sum+=maxx-heightMap[i+1][j];
				 res.push(make_tuple(heightMap[i+1][j], i+1, j));
				val[i+1][j]=1;
			}
			if(j+1<m and val[i][j+1]!=1)
			{
				if(heightMap[i][j+1]<maxx)
					sum+=maxx-heightMap[i][j+1];
				 res.push(make_tuple(heightMap[i][j+1], i, j+1));
				val[i][j+1]=1;
			}
			//cout<<sum<<endl;
		}
		return sum;

	}
};
```