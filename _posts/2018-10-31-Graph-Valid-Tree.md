---
layout: post
title:  "261. Graph Valid Tree"
date: 2018-10-31 22:19:23 -0400
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
class Solution {
public:
    bool validTree(int n, vector<pair<int, int>>& edges) {
        vector<vector<int>> TreeMap(n, vector<int>(n, -1));
        vector<int> visited(n, -1);
        for (int i = 0; i < edges.size(); i ++ ){
            int pre = edges[i].first;
            int post= edges[i].second;
            TreeMap[pre][post] = 1;
            TreeMap[post][pre] = 1;
            visited[pre] = 0;
            visited[post]= 0;
        }
        int count = 0;
        for (int i = 0; i < n; i++) { //遍历每个点，dfs每个点，看看从这个点会不会有圈
            if (visited[i] != 0) continue;
            queue<int> buffer;
            buffer.push(i); //判定这个i点有没有圈
            vector<int> temp = visited;
            while(!buffer.empty()){
                int cur = buffer.front();
                buffer.pop();
                if (temp[cur] == 1)
                    return false;
                temp[cur] = 1;
                count++;
                for (int j = cur; j < n; j++){
                    if (TreeMap[cur][j] == 1)
                        buffer.push(j);
                }
            }
        }
        if (count == n && edges.size() == n - 1)
            return true;
        return false;
    }
};
```