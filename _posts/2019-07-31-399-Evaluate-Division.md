---
layout: post
title:  "399. Evaluate Division"
date: 2019-07-31 22:12:00 -0400
categories: articles
---
Equations are given in the format A / B = k, where A and B are variables represented as strings, and k is a real number (floating point number). Given some queries, return the answers. If the answer does not exist, return -1.0.

Example:
```
Given a / b = 2.0, b / c = 3.0.
queries are: a / c = ?, b / a = ?, a / e = ?, a / a = ?, x / x = ? .
return [6.0, 0.5, -1.0, 1.0, -1.0 ].
```
The input is: vector<pair<string, string>> equations, vector<double>& values, vector<pair<string, string>> queries , where equations.size() == values.size(), and the values are positive. This represents the equations. Return vector<double>.

According to the example above:
```
equations = [ ["a", "b"], ["b", "c"] ],
values = [2.0, 3.0],
queries = [ ["a", "c"], ["b", "a"], ["a", "e"], ["a", "a"], ["x", "x"] ]. 
```
The input is always valid. You may assume that evaluating the queries will result in no division by zero and there is no contradiction.


```c++
class Solution {
public:
    const double EPS = 1e-6;
    double sol(unordered_map<string, unordered_map<string, double> >& graph, unordered_set<string>& visited, string& s, string& e) {
        if(s == e && (graph.find(s) != graph.end())){
            return 1.0;
        }
        double res = 0;
        for(auto item : graph[s]) {
            string s_new = item.first;
            if(visited.find(s_new) != visited.end()) continue;
            visited.emplace(s_new);
            res  = graph[s][s_new] * sol(graph, visited, s_new,e);
            if (res > 0) return res;
        }
        
        return -1.0;
    }
    
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_map<string, unordered_map<string, double> > graph;
        for(int i = 0; i < equations.size(); ++i) {
            string s = equations[i][0];
            string e = equations[i][1];
            graph[s][e] = values[i];
            if(values[i] == 0) graph[e][s] = -1.0;
            else graph[e][s] = 1.0/ values[i];
        }
        
        vector<double> res_list;
        for (auto item : queries) {
            unordered_set<string> visited;
            double res = sol(graph, visited, item[0], item[1]);
            res_list.emplace_back(res);
        }
        
        return res_list;
        
    }
};
```
```c++
class Solution {
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        vector<double> res;
        unordered_map<string,unordered_map<string, double>> m;
        for(int i = 0; i < equations.size(); i++){
            m[equations[i][0]][equations[i][1]] = values[i];
            m[equations[i][1]][equations[i][0]] = 1.0/values[i];
        }
        for(auto query : queries){
            unordered_set<string> visited;
            double t = dfs(m, query[0], query[1], visited);
            // res.push_back((t>0.0) ? t : -1);
            res.push_back(t);
        }
        return res;
    }
    double dfs(unordered_map<string,unordered_map<string, double>>& m, string start, string end, unordered_set<string>& visited){
        if(m[start].count(end)) return m[start][end];
        for(auto p : m[start]){
            if(visited.count(p.first)) continue;
            visited.insert(p.first);
            double t = dfs(m, p.first, end, visited);
            if(t > 0.0) 
                return t * p.second;
        }
        return -1.0;
    }
};
```

```c++
class Solution {
private:
    pair<string, double>& find(string& var, unordered_map<string, pair<string, double>>& root){
        // No record, create a new record for it
        if(root.find(var) == root.end())
            return root[var] = make_pair(var, 1.0);
        
        // It has itself as the root
        auto &origInfo = root[var];
        if(origInfo.first == var)
            return origInfo;
        
        // var = origRoot / origQ = newRoot / (origQ * newQ)
        double origQ = origInfo.second;
        auto &newInfo = find(origInfo.first, root);
        return root[var] = make_pair(newInfo.first, newInfo.second * origQ);
    }
    
public:
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        unordered_map<string, pair<string, double>> root;
        unordered_set<string> zero;
        
        // Make graph
        int nEquations = equations.size();
        for(int i = 0; i < nEquations; ++i){
            auto &vars = equations[i];
            string var0 = vars[0];
            string var1 = vars[1];
            double q = values[i];
            
            // var0 is zero
            if(q == 0){
                zero.insert(var0);
                continue;
            }
            
            // Retrieve root info for var0
            auto &info0 = find(var0, root);
            string root0 = info0.first;
            double q0 = info0.second;  // var0 = root0 / q0
            
            // Retrieve root info for var1
            auto &info1 = find(var1, root);
            string root1 = info1.first;
            double q1 = info1.second;  // var1 = root1 / q1
            
            // Update root info
            // root1 / q1 = var1 = var0 / q = root0 / (q * q0)
            //  => root1 = root0 / (q * q0 / q1)
            root[root1] = make_pair(root0, q * q0 / q1);
        }
        
        vector<double> ans;
        for(auto &query : queries){
            string var0 = query[0];
            string var1 = query[1];
            
            // var0 is zero
            if(zero.find(var0) != zero.end()){
                ans.push_back(0.0);
                continue;
            }
            
            // var0 or var1 doesn't appear before
            if(root.find(var0) == root.end() || root.find(var1) == root.end()){
                ans.push_back(-1.0);
                continue;
            }
            
            // var0 and var1 doesn't have the same root
            auto &info0 = find(var0, root);
            auto &info1 = find(var1, root);
            if(info0.first != info1.first){
                ans.push_back(-1.0);
                continue;
            }
            
            // var0 / var1 = (root0 / q0) / (root0 / q1) = q1 / q0
            double q0 = info0.second;
            double q1 = info1.second;
            ans.push_back(q1 / q0);
        }
        
        return ans;
    }
};
```