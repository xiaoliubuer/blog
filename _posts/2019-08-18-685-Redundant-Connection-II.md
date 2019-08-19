---
layout: post
title:  "685. Redundant Connection II"
date: 2019-08-18 17:46:00 -0400
categories: articles
---
In this problem, a rooted tree is a directed graph such that, there is exactly one node (the root) for which all other nodes are descendants of this node, plus every node has exactly one parent, except for the root node which has no parents.

The given input is a directed graph that started as a rooted tree with N nodes (with distinct values 1, 2, ..., N), with one additional directed edge added. The added edge has two different vertices chosen from 1 to N, and was not an edge that already existed.

The resulting graph is given as a 2D-array of edges. Each element of edges is a pair [u, v] that represents a directed edge connecting nodes u and v, where u is a parent of child v.

Return an edge that can be removed so that the resulting graph is a rooted tree of N nodes. If there are multiple answers, return the answer that occurs last in the given 2D-array.

Example 1:
```
Input: [[1,2], [1,3], [2,3]]
Output: [2,3]
Explanation: The given directed graph will be like this:
  1
 / \
v   v
2-->3
```
Example 2:
```
Input: [[1,2], [2,3], [3,4], [4,1], [1,5]]
Output: [4,1]
Explanation: The given directed graph will be like this:
5 <- 1 -> 2
     ^    |
     |    v
     4 <- 3
```
Note:
```
The size of the input 2D-array will be between 3 and 1000.
Every integer represented in the 2D-array will be between 1 and N, where N is the size of the input array.
```
```c++
class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n+1, 0), candA, candB;
        // step 1, check whether there is a node with two parents
        for (auto &edge:edges) {
            if (parent[edge[1]] == 0)
                parent[edge[1]] = edge[0]; 
            else {
                candA = {parent[edge[1]], edge[1]};
                candB = edge;
                edge[1] = 0;
            }
        } 
        // step 2, union find
        for (int i = 1; i <= n; i++) parent[i] = i;
        for (auto &edge:edges) {
            if (edge[1] == 0) continue;
            int u = edge[0], v = edge[1], pu = root(parent, u);
            // Now every node only has 1 parent, so root of v is implicitly v
            if (pu == v) {
                if (candA.empty()) return edge;
                return candA;
            }
            parent[v] = pu;
        }
        return candB;
    }
private:
    int root(vector<int>& parent, int k) {
        if (parent[k] != k) 
            parent[k] = root(parent, parent[k]);
        return parent[k];
    }
};
```
```c++
class Solution {
public:
    /*
    三种情况：
    第一种：无环，但是有结点入度为2的结点
    第二种：有环，没有入度为2的结点
    第三种：有环，且有入度为2的结点
    对于第一种情况，我们返回的产生入度为2的后加入的那条边，对于第二种情况，我们返回的是刚好组成环的最后加入的那条边，最后对于第三种情况我们返回的是组成环，且组成入度为2的那条边。
    */
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        vector<int> root(edges.size()+1, 0);
        vector<int> first, second;
        for(auto &e:edges) {
            if(root[e[1]]==0) root[e[1]]=e[0];//parent is unvisited
            else {//find the node with in degree=2 
                first={root[e[1]], e[1]};//root[e[1]] is e[1]'s parent.组成环，且组成入度为2的那条边
                second=e;//产生入度为2的后加入的那条边
                e[1]=0;//e[1]'s parent become 0, which means cutting off the edge e[0]->e[1]'
            }
        }
        for(int i=0; i<=edges.size(); i++) root[i]=i;//Every node has itself as its parent
        for(auto e:edges) {
            if(e[1]==0) continue;//no parent
            int x=getRoot(root, e[0]), y=getRoot(root, e[1]);
            if(x==y) return first.empty()?e:first;//e:return the answer that occurs last, first: 
            root[x]=y;
        }
        return second;
    }
    
    int getRoot(vector<int>& root, int idx) {
        return root[idx]==idx?idx:getRoot(root, root[idx]);
    }
};
```
```c++
class Solution {
public:
    int find(vector<int>& parent, int x) {
        if (parent[x] == x) return x;
        return parent[x] = find(parent, parent[x]);
    }
    
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        vector<int> parent(n + 1, 0), candA, candB;
        
        for (int i = 0; i < edges.size(); i++) {
            int u = edges[i][0], v = edges[i][1];
            if (parent[v] == 0) parent[v] = u;
            else {
                candA = {parent[v], v};
                candB = edges[i];
                edges[i][1] = 0;
            }
        }
        
        // union find
        for (int i = 1; i <= n; i++) parent[i] = i;
        for (int i = 0; i < edges.size(); i++) {
            int u = edges[i][0], v = edges[i][1];
            if (v == 0) continue;
            int root = find(parent, u);
            if (root == v) {
                if (candA.empty()) {
                    return edges[i];
                }
                return candA;
            }
            parent[v] = root;
        }
        
        return candB;
    }
};
```
```c++

class Solution {
public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        vector<int>res;
        vector<int>a, b;
        int n = edges.size();
        vector<int>roots(n+1, 0);
        for (auto &e: edges) {
            if (roots[e[1]] != 0) {
                a = {roots[e[1]], e[1]};
                b = e;
                e[1] = 0;
            }
            else 
                roots[e[1]] = e[0];
        }
        
        for (int i = 0; i < n; i++)
            roots[i] = i;
        for (auto &e: edges) {
            if (e[1] == 0) continue;
            int p = find(roots, e[0]);
            if (p == e[1]) {
                if (a.empty()) return e;
                return a;
            }
            roots[e[1]] = e[0];
        }
        return b;
    }
private: 
    int find(vector<int>&roots, int node) {
        if (roots[node] != node) roots[node] = find(roots, roots[node]);
        return roots[node];
    }
};
```
```c++
class Solution {
public:
    vector<int> root;
    
    int par(int x){
        while(root[x]!=x) {
            x = root[x];
        }
        return x;
    }
    
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int n = edges.size();
        root.assign(n+1,0);
        
        vector<int> res;
        vector<int> cand;
        
        for(int i=0;i<n;i++){
            root[i+1]=i+1;
        }
        
        for(auto i:edges){
            int u = i[0];
            int v = i[1];
            
            int pu = par(u);
            int pv = par(v);
            
            if(root[v] != v){
                //if multi parents after a loop, it means the new one should stay
                if(!cand.empty()){
                    res = {root[v],v};
                }
                //only multiple parents implies the second one is the ans
                else{
                    res = {u, v};
                    cand = {root[v], v};
                }
            }
            else if(pu == pv){
                //only loop, candidate is the ans
                if(cand.empty()) cand = {u, v};
                //loop after multi-parent implies the candidate is the culprit
                else res = cand;
            }
            if(root[v]==v && pu!=pv){
                root[v] = u;
            }
        }
        return res.empty()?cand:res;
    }
};
```