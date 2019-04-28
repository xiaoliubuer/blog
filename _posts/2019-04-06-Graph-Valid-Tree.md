---
layout: post
title:  "261. Graph Valid Tree"
date: 2019-04-06 18:04:01 -0400
categories: articles
---
Given n nodes labeled from 0 to n-1 and a list of undirected edges (each edge is a pair of nodes), write a function to check whether these edges make up a valid tree.

Example 1:

Input: n = 5, and edges = [[0,1], [0,2], [0,3], [1,4]]
Output: true
Example 2:

Input: n = 5, and edges = [[0,1], [1,2], [2,3], [1,3], [1,4]]
Output: false
Note: you can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0,1] is the same as [1,0] and thus will not appear together in edges.

啥意思呢？？就是有n个label，标记成了 0 到 n-1，是个无向连接。 下一个函数检测这一堆点是不是能够组成一个valid的树

所以什么是valid tree？？ **无环 + 全连接**

好的如何检测**无环**？那就 DFS来搜呗。
如何检测**全连接**？ edges = nodes - 1

# 思路：
因为是个无向图，所以就有点意思。
```c++
class Solution {
public:
	int Find(int v, vector<int>& parent){
		while( v != parent[v] ){
			v = parent[v];
		}
		return v;
	}

	void Union(int v1, int v2, vector<int>& parent){
		int p = Find(v1, parent);
		parent[p] = v2;
	}

    bool validTree(int n, vector<pair<int, int>>& edges) {
    	vector<int> parent(n);
    	for (int i = 0; i < parent.size(); ++i){ //初始化parent数组
    		parent[i] = i;
    	}
    	for (int i = 0; i < edges.size(); ++i){
    		int parent1 = edges[i].first;
    		int parent2 = edges[i].second;
    		if (Find(parent1, parent) == Find(parent2, parent))
    			return false;
    		else 
    			Union(parent1, parent2, parent);
    	}
    	return edges.size() == n-1;
    }
};
```

```c++
// BFS
// Look at notebook 1 to review the sovling process.
class Solution {
public:
    bool validTree(int n, vector<pair<int, int>>& edges) {
        if ( n != edges.size() + 1) return false;
        vector<vector<int>> path(n, vector<int>(n, 0));
        vector<int> visited(n, 0);
        queue<int> buffer;
        for (auto i : edges) {
            path[i.first][i.second] = 1;
            path[i.second][i.first] = 1;
            if ( buffer.empty() ) buffer.push(i.first);
        }
        while (!buffer.empty()) {
            int front = buffer.front();
            visited[front] = -1;
            buffer.pop();
            for (int i = 0; i < path[front].size(); i++) {
                if ( i != front && path[front][i] == 1){
                    if(visited[i] == 1) return false;
                    else {
                        if (visited[i] == 0) {
                            buffer.push(i);
                            visited[i] = 1;
                        }
                    }
                }
            }
        }
        return true;
    }
};
```
```c++
// Accepted!  2019-04-06
// Union Find
class Solution {
public:
    
    int Find(int val, vector<int>& parent){
        while (val != parent[val]) val = parent[val];
        return val;
    }
    
    void Union(int p1, int p2, vector<int>& parent){
        parent[p1] = p2;
    }

    bool validTree(int n, vector<pair<int, int>>& edges) {
        vector<int> parent(n);
        for (int i = 0 ; i < n; i++){
            parent[i] = i;
        }
        
        for (int i = 0; i < edges.size(); i++){
            int node1 = Find(edges[i].first, parent);
            int node2 = Find(edges[i].second,parent);
            
            if ( node1 == node2 ) return false;
            else{
                Union(node1, node2, parent);
                n--;
            }
        }
        return n == 1 ? true : false;
    }
};
```
```c++
//DFS
class Solution {
public:
    bool findCircle(int idx, int pre, vector<vector<int>>& path, vector<int>& visited){
        if ( visited[idx] == 1 ) return true;
        
        visited[idx] = 1;
        for (int i = 0; i < path[idx].size(); i++ ) {
            if (i != idx && i != pre && path[idx][i] == 1 ){
                if (findCircle(i, idx, path, visited)) return true;
            }
        }
        visited[idx] = -1; // Important!!!
        return false;
    }
    bool validTree(int n, vector<pair<int, int>>& edges) {
        vector<vector<int>> path(n, vector<int>(n,0));
        
        for (auto& i : edges){
            path[i.first][i.second] = 1;
            path[i.second][i.first] = 1;
        }
        
        vector<int> visited(n, 0);
        for (int i = 0; i < n; i++) {
            if(visited[i] == 0 && findCircle(i, i,path, visited)) return false; // visited[i] == 0 Important!!!
        }
        return n == edges.size() + 1;
    }
};
```