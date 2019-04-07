---
layout: post
title:  "207. Course Schedule"
date: 2019-04-06 17:39:01 -0400
categories: articles
---
There are a total of n courses you have to take, labeled from 0 to n-1.

Some courses may have prerequisites, for example to take course 0 you have to first take course 1, which is expressed as a pair: [0,1]

__Given the total number of courses and a list of prerequisite pairs, is it possible for you to finish all courses?__

Example 1:

Input: 2, [[1,0]] 
Output: true
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0. So it is possible.
Example 2:

Input: 2, [[1,0],[0,1]]
Output: false
Explanation: There are a total of 2 courses to take. 
             To take course 1 you should have finished course 0, and to take course 0 you should
             also have finished course 1. So it is impossible. （不存在环）

# 思路1:

问题问的是有没有可能上完所有的课？

这句话又是什么意思呢？ 

1. 就是所有的点都是相连的。
2. 这里不存在环。

那么怎么判定这里的所有的点都是相连的呢？？

不不不，有没有可能一门课的prerequisites有不止一门课？？当然应该是有的～～

所以我们那判定的条件应该是。这个有向图。所以呢？？
有向图就会有 outdegree and indegree。
所以呢？？

所以就是，如果我们从找从一个indegree为0的点进行搜索，找到他，就把它删除掉。然后它相连的所有点的indegree -1
然后继续找indegree为0的点，然后一直这里下去。到最后没有办法进行删除的时候。我们来判断。

所删除的点是不是 等于 原来 舒服的n个点。如果是，那就是如果

等于下，如果有环怎么办。有环的话，就没有办法找到indegree为0的点了。那就回早早收场。
那么随后弹出去的点就不够那么多。

**所以最后就是比较： 输入的 N 个点，与复合条件弹出去的点的个数是不是想等。**

**那个是解法就是BFS，广度优先搜索。**


所以就是，需要两个东西:
1. 一个是记录几个点有几个 indegree的 vector。 
2. 记录这个点走向➡️的所有点。 vector<vector<int>> 

还要有一个 counter 记录 pop() 出去了几个点。

然后用 queue，的广度优先遍历。找到那个indegree为0的点，把它push到queue里面，然后对于
queue进行判空操作。拿到front，然后对这个点的所有连接的点进行 ➖ indegree，发现➖完之后的indegree等于0的时候就push到queue里面。以此往复。直至结束。

最后对比 return counter == numCourses;


```c++
// BFS 用indegree的方法进行搜索
class Solution {
public:
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        if (numCourses == 0 && prerequisites.size() != 0) return false;
        vector<int> indegree(numCourses, 0);
        vector<vector<int>> matrix(numCourses, vector<int>());
        // Construct the relationship about the connection of the nodes.
        for (int i = 0; i < prerequisites.size(); i++) {
            int post = prerequisites[i].first;
            int pre  = prerequisites[i].second;
            indegree[post]++;
            matrix[pre].push_back(post);
        }
        queue<int> buffer;
        //Push the indegree 0 node into buffer
        for ( int i = 0; i < indegree.size(); i++ ) {
            if (indegree[i] == 0)
                buffer.push(i);
        }
        // Init a counter to caculate numbers of poped nodes
        int counter = 0;
        //BFS
        while (!buffer.empty()) {
            int front = buffer.front();
            buffer.pop();
            counter++;
            for (int i = 0; i < matrix[front].size(); i++){
                indegree[matrix[front][i]]--;
                if (indegree[matrix[front][i]] == 0) buffer.push(matrix[front][i]);
            }
        }
        return counter == numCourses;
    }
};
```

# 思路2:
如果从一个点出发，进行搜索，如果就沿着它能指向的方向走。如果走到一个点，发现这个地方是以前来过的。那么这个地方就是环。如果发现环，就说明有问题。
但是通过每个点的走，最后都没有发现走成环。那么这个结构就可以。

具体怎么来做呢？？？ 这么用DFS。
DFS就是从一个点开始，沿着它指向的就一直往下搜。如果又回到原来的一个点，那就是有问题。

所以我要创造一个 N * N的矩阵。

在loop prerequisite的时候，给矩阵进行初始化。

然后就对那个矩阵进行扫名

扫到一个，然后就跳到那一行，然后继续在扫到一个。我可能需要两个矩阵，进行扫的，一个是进行标记扫眉扫过的。

```c++
//DFS
// 我操，竟然一次过！！！！牛逼啦！！
class Solution {
public:
    // Helper founction is used to find cycle. If find it, return false, if not find, return true.
    bool helper(int idx, vector<vector<int>>& matrix, vector<int>& visited){
        if (visited[idx]) return false;
        visited[idx] = true;
        for (int i = 0; i < matrix[idx].size(); i++ ){
            if (!helper(matrix[idx][i], matrix, visited)) return false;
        }
        visited[idx] = false;
        return true;
    }
    
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        if ( numCourses == 0 && prerequisites.size() != 0 ) return false;

        // DFS's aim is to find the cycle.
        vector<int> visited(numCourses, false);
        // Used to construct relationship of the nodes.
        vector<vector<int>> matrix(numCourses, vector<int>());
        // Init the 
        for (int i = 0; i < prerequisites.size(); i++) {
            int post = prerequisites[i].first;
            int pre  = prerequisites[i].second;
            matrix[pre].push_back(post);
        }
        // Front first node start to do the DFS
        for (int i = 0; i < numCourses; i++){
            if (!helper(i, matrix, visited)) return false;
        }
        return true;
    }
};
```

**如果要用到helper函数的话，一定要明确这个helper函数的作用是什么。他是干什么用的，返回值是什么。这里很重要，这样就不会让我们对helper founction迷惑啦～～**


```c++
// Accepted! 2019-04-03
// DFS
class Solution {
public:
    
    bool find_circle(vector<vector<int>>& path, int i, vector<bool>& visited){
        if (visited[i]) return true;
        visited[i] = true;
        for (int j = 0; j < path[i].size(); j++){
                if (find_circle(path, path[i][j],visited)) return true;
        }
        visited[i] = false;
        return false;
    }
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<vector<int>> path(numCourses);
        vector<bool> visited(numCourses, false);
        for (auto i : prerequisites) {
            int pre = i.second;
            int post= i.first;
            path[pre].push_back(post);
        }
        for (int i = 0; i < numCourses; i++){
            if (find_circle(path, i, visited)) return false;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    
    bool findCircle(vector<vector<int>>& path, int idx, vector<bool>&  visited){
        if (visited[idx]) return true;
        visited[idx] = true;
        for (int i = 0; i < path[idx].size(); i++){
            if (findCircle(path, path[idx][i], visited)) return true;
        }
        visited[idx] = false;
        return false;
    }
    bool canFinish(int numCourses, vector<pair<int, int>>& prerequisites) {
        vector<bool>  visited(numCourses, false);
        vector<vector<int>> path(numCourses);
        for (auto i : prerequisites){
            int pre = i.second;
            int post= i.first;
            path[pre].push_back(post);
        }
        
        for (int i = 0; i < numCourses; i++){
            if (findCircle(path, i, visited)) return false;
        }
        return true;
    }
};
```