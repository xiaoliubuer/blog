---
layout: post
title:  "547. Friend Circles"
date: 2018-12-27 07:20:23 -0400
categories: articles
---
There are N students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a N * N matrix M representing the friend relationship between students in the class. If M[i][j] = 1, then the ith and jth students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

Example 1:
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
Example 2:
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
Note:
N is in range [1,200].
M[i][i] = 1 for all students.
If M[i][j] = 1, then M[j][i] = 1.

题意:
有N个学生，他们有些是朋友，有些不是，他们之间的关系是可以传递的，比如：A是B的直接朋友，B是C的直接朋友，那么A和C就是间接朋友，我们把是直接或间接的朋友算作一个朋友圈。

给一个N * N的矩阵M表示他们之间的关系。

# 思路：

# 方法一：Union Find
用Union Find的话可能还得构造边的关系。现在给的是矩阵，那么如何得到两个点的关系？？
其实不用建立pair也可以，就是遍历矩阵，把i和j传进去进行Find和Union就行了
1. Function Signature
```c++
class Solution {
public:
	int findCircleNum(vector<vector<int>>& M) {
		return 0;
	}
};
```
2. Corner Case:
```c++
if ( M.size() == 0 || M[0].size() == 0 ) return 0;
int res = M.size();
```
3. Define visited
```c++
vector<int> root;
for (int i = 0; i < M.size(); i++)
	root.push_back(i);
```
4. Traverse matrix to use Find and Union mehtod
```c++
	for (int i = 0; i < M.size(); i++) {
		for (int j = 0; j < M[0].size(); j++) {
			if (M[i][j] == 1){
			int first = Find(root, i);
			int second= Find(root, j);
			if ( first != second ) {
				res--;
				Union(root, first, second);
			}
		}
		}
	}
```
5. Return res
```C++
		return res;
```
6. Find mehtod
```c++
		int Find(vector<int> root, int node){
			while( node != root[node])
				node = root[node];
			return node;
		}
```
7. Union method
```c++
	void Union(vector<int>& root, int first, int second){
		int idx = Find(root, first);
		root[second] = idx;
	}
```

# 方法二：DFS(Iteration)
就是从一个点开始搜，找他的所有的相关联的点，如果找到了就把总体-1.
DFS的总体实现方法有两种，Iteration 和 Recursion

Iteration方法：
1. Init Visited标记
```c++
int n = M.size();
vector<int> visited(n, 0); //0 means not visited yet
```
2. 遍历所有的点，然后依据每个点去寻找他们的component,因为是iteration，所以用到stack
```c++
stack<int> buff;
int res = 0;
for (int i = 0; i < n; i++) {
	if (visited[i] == 0) {
		buff.push(i);
		res++;
	}
	//对于node i进行的iteration DFS
	while(!buff.emtpy()){
		int top = buff.top();
		buff.pop();
		for (int j = top; j < n; j++ ) {
			if (M[top][j] == 1 && visited[j] == 0){
				buff.push(j);
				visited[j] = 1;
			}
		}
	}
}
```
3. 返回res
```c++
	return res;
```

# 方法三：DFS(Recursion)
对于要用到Recursion的DFS，首先一定要明确helper function是用来干嘛的！！
helper function就是用来把每个点放进去，然后找个点的深度优先的所有相连点。

1. 所以要首先遍历每个人, 对每个人进行DFS recursion
```c++
int res = 0;
vector<int> visited(n,0);
int n = M.size();
for (int i = 0; i < n; i++ ){
	if ( visited[i] == 0 ) {
		helper(M, visited, i); //这里涉及到一个对于矩阵的DFS算法
		res++;
	}
}
```
2. helper function
目的就是为了把这个点的所有相关联的所有visited都标记成1
```c++
	void helper(vector<vector<int>>& M, vector<int>& visited, int node){
		visited[node] = 1;
		for (int i = 0; i < M.size(); i++) {
			if ( M[node][i] == 1 && visited[i] == 0 )
				helper(M, visited, i);
		}
	}
```
3. return res
```c++
return res;
```

# 参考答案
```c++
//Union Find
class Solution {
public:
    int Find(vector<int> root, int node){
        while( node != root[node])
            node = root[node];
        return node;
    }
	void Union(vector<int>& root, int first, int second){
        int idx = Find(root, first);
        root[second] = idx;
    }
    int findCircleNum(vector<vector<int>>& M) {
        if ( M.size() == 0 || M[0].size() == 0 ) return 0;
        int res = M.size();
        vector<int> root;
        for (int i = 0; i < M.size(); i++){
            root.push_back(i);
        }
        
        for (int i = 0; i < M.size(); i++) {
            for (int j = 0; j < M[0].size(); j++) {
                if (M[i][j] == 1){
                    int first = Find(root, i);
                    int second= Find(root, j);
                    if ( first != second ) {
                        res--;
                        Union(root, first, second);
                    }
                }
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
    int findCircleNum(vector<vector<int>>& M) {
        if (M.size() == 0 || M[0].size() == 0) return 0;
        int n = M.size();
        vector<int> visited(n, 0); //0 means not visited yet
        stack<int> buff;
        int res = 0;
        for (int i = 0; i < n; i++) {
            if (visited[i] == 0) {
                buff.push(i);
                res++;
            }
            //对于node i进行的iteration DFS
            while(!buff.empty()){
                int top = buff.top();
                buff.pop();
                for (int j = 0; j < n; j++){
                    if (M[top][j] == 1 && visited[j] == 0){
                        buff.push(j);
                        visited[j] = 1;
                    }
                }
            }
        }
        return res;
    }
};
```

```c++
// DFS Recursion
class Solution {
public:
	void helper(vector<vector<int>>& M, vector<int>& visited, int node){
		visited[node] = 1;
		for (int i = 0; i < M.size(); i++) {
			if ( M[node][i] == 1 && visited[i] == 0 )
				helper(M, visited, i);
		}
	}
	int findCircleNum(vector<vector<int>>& M) {
		if (M.size() == 0 || M[0].size() == 0) return 0;
		int res = 0;
		int n = M.size();
		vector<int> visited(n,0);
		for (int i = 0; i < n; i++ ){
			if ( visited[i] == 0 ) {
			helper(M, visited, i); //这里涉及到一个对于矩阵的DFS算法
			res++;
			}
		}
        return res;
    }
};
```