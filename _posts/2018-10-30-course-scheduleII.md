---
layout: post
title:  "210. Course Schedule II"
date: 2018-10-30 22:46:23 -0400
categories: articles
---

There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

Given the total number of courses and a list of prerequisite pairs, return the ordering of courses you should take to finish all courses.

There may be multiple correct orders, you just need to return one of them. If it is impossible to finish all courses, return an empty array.

Example 1:

Input: 2, [[1,0]] 
Output: [0,1]
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished   
             course 0. So the correct course order is [0,1] .
Example 2:

Input: 4, [[1,0],[2,0],[3,1],[3,2]]
Output: [0,1,2,3] or [0,2,1,3]
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both     
             courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0. 
             So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3] .
Note:

1. The input prerequisites is a graph represented by a list of edges, not adjacency matrices. Read more about how a graph is represented.
2. You may assume that there are no duplicate edges in the input prerequisites.

这道题和207的区别是什么呢？？ 前一到只是 true or false，

这一道就是要把整顺序打印出来。

```c++
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<int> res;
        vector<vector<int>> path(numCourses);
        vector<int> indegree(numCourses, 0);
        for (int i = 0; i < prerequisites.size(); ++i){
            int post= prerequisites[i].first;
            int pre = prerequisites[i].second;

          indegree[post]++;//这里很重要！！！！
          path[pre].push_back(post);
        }

        queue<int> myqueue;
        for (int i = 0; i < indegree.size(); ++i){
          if (indegree[i] == 0)
            myqueue.push(i);
        }

        int sum = 0;
        while(!myqueue.empty()){
          int temp = myqueue.front();
          myqueue.pop();
          res.push_back(temp);
          sum++;

          for (int i : path[temp]){
            indegree[i]--;
            if (indegree[i] == 0) myqueue.push(i);
          } 
        }
    return sum == numCourses ? res : vector<int>();
    }
};
```

来，我来试试DFS，DFS是一只要把一条路径找完，然后在找另一个点的一条路径
```c++
// DFS 深度优先有点问题
// 关键就是那个点如何插入，这个是个问题，这里会造成很慢的操作，插入一个节点都会成为N的操作。
// 所以DFS这个地方如何优化还需要考虑
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<pair<int, int>>& prerequisites) {
    	vector<int> res;
    	if (numCourses == 0 || prerequisites.size() == 0) return res;
    	
    }
};
```