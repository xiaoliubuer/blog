

---
layout: post
title:  "690. Employee Importance"
date: 2019-08-18 18:00:00 -0400
categories: articles
---

You are given a data structure of employee information, which includes the employee's unique id, his importance value and his direct subordinates' id.

For example, employee 1 is the leader of employee 2, and employee 2 is the leader of employee 3. They have importance value 15, 10 and 5, respectively. Then employee 1 has a data structure like [1, 15, [2]], and employee 2 has [2, 10, [3]], and employee 3 has [3, 5, []]. Note that although employee 3 is also a subordinate of employee 1, the relationship is not direct.

Now given the employee information of a company, and an employee id, you need to return the total importance value of this employee and all his subordinates.

Example 1:
```
Input: [[1, 5, [2, 3]], [2, 3, []], [3, 3, []]], 1
Output: 11
Explanation:
Employee 1 has importance value 5, and he has two direct subordinates: employee 2 and employee 3. They both have importance value 3. So the total importance value of employee 1 is 5 + 3 + 3 = 11.
```
Note:
```
One employee has at most one direct leader and may have several subordinates.
The maximum number of employees won't exceed 2000.
```
```c++
class Solution {
public:
    map<int,Employee*> imap;
    
    int getImportance(vector<Employee*> employees, int id) {
        for(Employee* E: employees){
            imap[E->id]=E;
        }
        return dfs(id);
    }
    
    int dfs(int id) {
        Employee* E=imap[id];
        int s=E->importance;
        for(int i: E->subordinates){
            s+=dfs(i);
        }
        return s;
    }
};
```
```c++
class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        queue<Employee*> q;
        unordered_map<int, Employee*> map;
        for(Employee* e : employees)
            map[e->id] = e;
        q.push(map[id]);
        int res = 0;
        Employee* temp = NULL;
        while(!q.empty())
        {
            temp = q.front();
            q.pop();
            res += temp->importance;
            for(int i = 0; i < temp->subordinates.size(); i++)
                q.push(map[temp->subordinates[i]]);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int getImportance(vector<Employee*> employees, int id) {
        unordered_map<int,Employee*> m;        
        deque<int> S;
        Employee *e;
        int i, sum = 0;
        for(i = 0; i < employees.size(); ++i)
            m[employees[i]->id] = employees[i];
        S.push_back(id);
        while(S.size()) {
            e = m[S.back()]; S.pop_back();
            sum += e->importance;
            for(i = 0; i < e->subordinates.size(); ++i)
                S.push_back(e->subordinates[i]);
        } return sum;
    }
};
```
```c++
class Solution {
public:
    
    map<int,Employee*> hash;
    
    int getImportance(vector<Employee*> employees, int id) {
        for(auto e:employees){
            hash[e->id]=e;
        }
        return dfs(id);
    }
    
    int dfs(int id){
        Employee* e = hash[id];
        int ans = e->importance;
        for(auto s:e->subordinates){
            ans+=dfs(s);
        }
        return ans;
    }
};
```

```c++
class Solution {
public:

int getImportance(vector<Employee*> employees, int id) {
    unordered_map<int,int> eid, val;
    int idx = 1;
    
    // eid stores map of id to index
    // val stores map of id to importance
    for (auto i: employees) {
        if (eid[i->id] == 0) {
            eid[i->id] = idx++;
            val[i->id] = i->importance;
        }
    }
    
    int n = eid.size();
    
    // adjacency matrix for dfs
    vector< vector<int> > graph(n+1);
    
    // for each employee, add its subordinates as its neighbours
    for (auto i: employees) {
        for (int j=0; j<i->subordinates.size(); ++j) {
            graph[eid[i->id]].push_back(i->subordinates[j]);
        }
    }
    
    int ans = dfs(graph,eid,val,id,0);
    
    return ans;
}

int dfs(vector< vector<int> >& graph, unordered_map<int,int>& eid, unordered_map<int,int>& val, int id, int x) {
    // add current employee's importance to answer
    x += val[id];
    
    // for all its subordinates,
    // call dfs and add to answer
    for (int j: graph[eid[id]]) {
        x += dfs(graph,eid,val,j,0);
    }
    
    return x;
}
};
```