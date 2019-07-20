---
layout: post
title:  "310. Minimum Height Trees"
date: 2019-06-23 17:02:00 -0400
categories: articles
---
For an undirected graph with tree characteristics, we can choose any node as the root. The result graph is then a rooted tree. Among all possible rooted trees, those with minimum height are called minimum height trees (MHTs). Given such a graph, write a function to find all the MHTs and return a list of their root labels.

Format
The graph contains n nodes which are labeled from 0 to n - 1. You will be given the number n and a list of undirected edges (each edge is a pair of labels).

You can assume that no duplicate edges will appear in edges. Since all edges are undirected, [0, 1] is the same as [1, 0] and thus will not appear together in edges.

Example 1 :
```
Input: n = 4, edges = [[1, 0], [1, 2], [1, 3]]

        0
        |
        1
       / \
      2   3 

Output: [1]
```
Example 2 :
```
Input: n = 6, edges = [[0, 3], [1, 3], [2, 3], [4, 3], [5, 4]]

     0  1  2
      \ | /
        3
        |
        4
        |
        5 

Output: [3, 4]
```
Note:
According to the definition of tree on Wikipedia: “a tree is an undirected graph in which any two vertices are connected by exactly one path. In other words, any connected graph without simple cycles is a tree.”
The height of a rooted tree is the number of edges on the longest downward path between the root and a leaf.

# Summary
Use any note as root of a tree, find all the node which make the tree is the MHT.

```c++
//BFS
class Solution {
public:
  vector<int> findMinHeightTrees(int n, vector<vector<int>>& edges) {
    unordered_map<int, unordered_set<int>> graph;
    for(const auto &e : edges) {
      graph[e[0]].insert(e[1]);
      graph[e[1]].insert(e[0]);
    }
    
    queue<int> que;
    for(int i = 0; i<n; ++i)
      if(graph[i].size() == 1) que.push(i);
    
    while(que.size() and graph.size()>2) {
      for(int i=que.size(); i>0; --i) {
        auto top = que.front(); que.pop();
        
        for(auto& next : graph[top]) {
          graph[next].erase(top);
          if(graph[next].size() == 1)
            que.push(next);
        }
        graph.erase(top);
      }
    }
    vector<int> res;
    for(const auto& node : graph)
      res.emplace_back(node.first);
    
    return res;
  }
};
```

```c++
class Solution {
public:
    void DFS(std::vector<std::vector<int>> const& graph,
             std::vector<bool>& visited,
             std::vector<int>& longestPath,
             std::vector<int>& dfsPath,
             int current)
    {
        dfsPath.push_back(current);
        visited[current] = true;

        bool isLeaf = true;
        for (auto node : graph[current]) {
            if (!visited[node]) {
                isLeaf = false;
                DFS(graph, visited, longestPath, dfsPath, node);
            }
        }

        if (isLeaf && dfsPath.size()>longestPath.size()) {
            longestPath.assign(dfsPath.begin(), dfsPath.end());
        }

        dfsPath.pop_back();
    }

    std::vector<int> findMinHeightTrees(int n, std::vector<std::vector<int>>& edges) {
        std::vector<std::vector<int>> graph(n, std::vector<int>());
        for (auto& edge : edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }

        // find the one end of the longest path
        std::vector<bool> visited(n, false);
        std::vector<int> dfsPath;
        std::vector<int> longestPath;
        DFS(graph, visited, longestPath, dfsPath, 0);
        int next = longestPath.back();

        // find the longest path
        visited.assign(n, false);
        dfsPath.clear();
        longestPath.clear();
        DFS(graph, visited, longestPath, dfsPath, next);

        std::vector<int> result;
        int m = longestPath.size();
        if (m%2 == 0) {
            result.push_back(longestPath[m/2-1]);
            result.push_back(longestPath[m/2]);
        } else {
            result.push_back(longestPath[(m-1)/2]);
        }

        return result;
    }

};
```