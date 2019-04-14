---
layout: post
title:  "323. Number of Connected Components in an Undirected Graph"
date: 2019-04-07 14:43:00 -0400
categories: articles
---
Given n nodes labeled from 0 to n - 1 and a list of undirected edges (each edge is a pair of nodes), write a function to find the number of connected components in an undirected graph.

Example 1:

Input: n = 5 and edges = [[0, 1], [1, 2], [3, 4]]

     0          3
     |          |
     1 --- 2    4 

Output: 2
Example 2:

Input: n = 5 and edges = [[0, 1], [1, 2], [2, 3], [3, 4]]

     0           4
     |           |
     1 --- 2 --- 3

Output:  1
Note:
You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

# 题意：
给了N个点被标为0到n-1，和一个存有无向边pair的list，写一个函数找到connected components的个数。

# 思路：

不要回忆！！
不要回忆！！！
现场分析！！现场分析！！！

# 方法一： DFS
遇到了无向图和边的关系. 深度优先遍历！找到一个点，然后找到和这个点所有相连的点。直到找不到为止。
然后遍历所有的这个已输入的点。最终找到有多少个connected component。
1. function signature
```c++
	class Solution {
	public:
	    int countComponents(int n, vector<pair<int, int>>& edges) {
	        return 0;
	    }
	};
```
2. corner case process
```c++
	if ( edges.size() == 0 ) return n;
```
3. convert edge format
为了方便，我们把这个list of pair转化成一个N乘N的矩阵。因为这样我们可以从一个点开始，对它所有相关联的点进行深度优先遍历。
```c++
	//init a matrix to store the connection relationship
	vector<vector<int> > matrix(n, vector<int>(n, 0)); 
	for (int i = 0; i < edges.size(); i++ ){
		int pre = edges[i].first;
		int post= edges[i].second;
		matrix[pre][post] = 1;
		matrix[post][pre] = 1;
	}
```
4. DFS
从0开始，进行dfs遍历，把这个点的相关的点都标注为visited的标记。注意！！这里有两个非常容易混淆的地方：
第一层循环是遍历每个点，顺序基本依次为：0，1，2，3，4，5... 第二层循环是遍历每个点的相关联的每个点，也就是比如0，[0,3], [0,5],之类的，也就是与0相连的点
```c++
	vector<int> visited(n, 0); // 0 means is not visted yet
	stack<int> buff;
	int counter = 0;
	//DFS还有两种，iteration 和 recursion
	for (int i = 0; i < n; i++ ) {
		if (visited[i] == 0) {
			buff.push();
			counter++;
		}
		while(!buff.empty()){
			int top = buff.top();
			buff.pop();
			visited[top] = 1;
			for (int j = i; j < matrix[i].size(); j++ ) {
				int temp = matrix[i][j];
				if ( visited[temp] == 0 ) buff.push(temp);
			}
		}
	}
```
5. return counter
```c++
return counter;
```
```c++
// Accepted!! 2019-04-07
// Recursion
class Solution {
public:
    
    void helper(vector<vector<int>>& path, int idx, vector<bool>& visited){
        if (visited[idx]) return;
        visited[idx] = true;
        for (int i = 0; i < path[idx].size(); i++){
            if ( path[idx][i] == 1){
                helper(path, i, visited);
            }
        }
    }
    int countComponents(int n, vector<pair<int, int>>& edges) {
        vector<vector<int>> path(n, vector<int>(n, 0));
        vector<bool> visited(n, false);
        for (auto i : edges) {
            path[i.first][i.second] = 1;
            path[i.second][i.first] = 1;
        }
        
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (visited[i] == false){
                helper(path, i, visited);
                res++;
            }
        }
     return res;   
    }
};
```
# 方法2: Union find
就是把来源于同样一个根的所有节点都连到一起
0. Funciton signature:
```c++
	class Solution {
	public:
	    int countComponents(int n, vector<pair<int, int>>& edges) {
	        return 0;	
	    }
	};
```
实现一个find function，用来对输入的一条边进行检验，确定这个边上的两个点是不是源于同样的一个起始点。
1. 初始化标记起始点的数组
```c++
	//最初，设置每个点的root就是他们自己
	if (edges.size() == 0) return n;
	int res = n;
	vector<int> root(n, 0);
	for (int i = 0; i < n; i++) {
		root[i] = i;
	}
```
2. 对现有的所有的边进行遍历，寻找他们是不是已经相连，如果相连不动，没有相连，那就连起来，总体个数-1.
```c++
	for (int i = 0; i < edges.size(); i++){
		int first = edges[i].first;
		int second= edges[i].second;
		if (find(root, first) != find(root, second)){
			union(root, edges[i]);
			res--;
		}
	}
```
3. 实现find function， find函数就是用来通过输入的一个边，来寻找这条边上的两个点能够追溯到同样的一个起点。
```c++
	//
	int find(vector<int> root, int node){
		while( root[node] != node){
			root[node] = node;
		}
		return node;
	}
```
4. 实现union function, union函数就是用来把两个点联系在一起的。
```c++
		void union(vector<int>& root, pair<int, int> edge){
			int first = edge.first;
			int second = edge.second;
			root[second] = first;
		}
```
5. 最后返回res
```c++
return res;
```

# 参考代码：
```c++
// DFS Recursion
class Solution {
public:
    void DFS(vector<vector<int>>& matrix, vector<bool>& visited, int idx){
        for (int i = 0; i < matrix.size(); ++i){
            if (matrix[idx][i] == 1 ){
                if(visited[i] == false){
                visited[i] = true;
                DFS(matrix, visited, i);
                }
            }

        }
    }
    int countComponents(int n, vector<pair<int, int>>& edges) {
    if (edges.size() == 0) return n;

    vector<bool> visited(n, false);
    vector<vector<int>> matrix(n, vector<int>(n, -1));

    for (int i = 0; i < edges.size(); ++i){
        int pre = edges[i].first;
        int pos = edges[i].second;
        matrix[pre][pos] = 1;
        matrix[pos][pre] = 1;
        }
    int res = 0;
    for (int i = 0; i < n; ++i){
        if ( visited[i] == false){
            res++;
            DFS(matrix, visited, i);
        }
    }
    return res;
    }
};
```
```c++
// DFS Iteration
class Solution {
public:
    int countComponents(int n, vector<pair<int, int>>& edges) {
        if ( edges.size() == 0 ) return n;
        vector<vector<int> > matrix(n, vector<int>(n, 0));
        for (int i = 0; i < edges.size(); i++) {
            int pre = edges[i].first;
            int post= edges[i].second;
            matrix[pre][post] = 1;
            matrix[post][pre] = 1;
        }
        int visited[n] = {0};
        int counter = 0;
        stack<int> buff; // queue<int> buff is OK here
        for (int i = 0; i < n; i++) {
            if (visited[i] == 0) {
                buff.push(i);
                counter++;
            }
            while(!buff.empty()) {
                int top = buff.top();
                buff.pop();
                visited[top] = 1;
                for (int j = 0; j < matrix[top].size(); j++ ) {
                    int temp = matrix[top][j];
                    if ( temp == 1 && visited[j] == 0) buff.push(j);
                }
            }
        }
        return counter;
    }
};
```
```c++
// Union Find
class Solution {
public:
    int Find(int val, vector<int>& parent){
        while (val != parent[val]) val = parent[val];
        return val;
    }
    void Union(int p1, int p2, vector<int>& parent){
        parent[p2] = p1;
    }
    int countComponents(int n, vector<pair<int, int>>& edges) {
        vector<int> parent(n);
        for (int i = 0; i < n; i++){
            parent[i] = i;
        }
        for (auto i : edges) {
            int p1 = Find(i.first, parent);
            int p2 = Find(i.second,parent);
            if ( p1 != p2 ) {
                Union(p1, p2, parent);
                n--; // Important!!!!
            }
        }
        return n;
    }
};
```