---
layout: post
title:  "785. Is Graph Bipartite?"
date: 2019-05-09 00:28:00 -0400
categories: articles
---

Given an undirected graph, return true if and only if it is bipartite.

Recall that a graph is bipartite if we can split it's set of nodes into two independent subsets A and B such that every edge in the graph has one node in A and another node in B.

The graph is given in the following form: graph[i] is a list of indexes j for which the edge between nodes i and j exists.  Each node is an integer between 0 and graph.length - 1.  There are no self edges or parallel edges: graph[i] does not contain i, and it doesn't contain any element twice.

Example 1:
Input: [[1,3], [0,2], [1,3], [0,2]]
Output: true
Explanation: 
The graph looks like this:
0----1
|    |
|    |
3----2
We can divide the vertices into two groups: {0, 2} and {1, 3}.
Example 2:
Input: [[1,2,3], [0,2], [0,1,3], [0,2]]
Output: false
Explanation: 
The graph looks like this:
0----1
| \  |
|  \ |
3----2
We cannot find a way to divide the set of nodes into two independent subsets.
 

Note:

graph will have length in range [1, 100].
graph[i] will contain integers in range [0, graph.length - 1].
graph[i] will not contain i or duplicate values.
The graph is undirected: if any element j is in graph[i], then i will be in graph[j].

给与自己相临的节点染与自己不同的颜色，如果发现自己的邻居和自己同色，那就说明不行，否则就可以。

```c++
// DFS
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, - 1); // color for each node
        
        for ( int start = 0; start < n; start++ ){ // Go over each node
            if ( color[start] == -1 ){
                stack<int> st;
                st.push(start);
                color[start] = 0;
                
                while(!st.empty()) {
                    int top = st.top();
                    st.pop();
                    for ( auto i : graph[top]){
                        if ( color[i] == -1 ) {
                            st.push(i);
                            color[i] = color[top] ^ 1;
                        }
                        else if ( color[i] == color[top]){
                            return false;
                        }
                    }
                }
            }
            
        }
        return true;
    }
};

//[[1,3], [0,2], [1,3], [0,2]]
//   0      1      2       3
// node 0 connect with 1, 3, node 1 connect with 0, 2, node 2 connect with 1,3, node 3 with 0, 2
// Means:
```

```c++
// BFS
class Solution {
public:
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> color(n, -1); // num of node is n, color for each node
        
        for ( int start = 0; start < n; start++ ){ // Go over each node
            if ( color[start] == -1 ){ // Not visit yet.
                queue<int> st;
                st.push(start);
                color[start] = 0; // color 0
                
                while(!st.empty()) { // DFS go through each node's connected node
                    int top = st.front();
                    st.pop();
                    for ( auto i : graph[top]){
                        if ( color[i] == -1 ) {
                            st.push(i);
                            color[i] = color[top] ^ 1; //XOR
                        }
                        else if ( color[i] == color[top]){
                            return false;
                        }
                    }
                }
            }
            
        }
        return true;
    }
};

//[[1,3], [0,2], [1,3], [0,2]]
//   0      1      2       3
// node 0 connect with 1, 3, node 1 connect with 0, 2, node 2 connect with 1,3, node 3 with 0, 2
// Means:
```