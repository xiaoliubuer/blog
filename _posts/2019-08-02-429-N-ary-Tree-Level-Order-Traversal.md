---
layout: post
title:  "429. N-ary Tree Level Order Traversal"
date: 2019-08-02 19:54:00 -0400
categories: articles
---
Given an n-ary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example, given a 3-ary tree:
We should return its level order traversal:
```
[
     [1],
     [3,2,4],
     [5,6]
]
```
Note:

The depth of the tree is at most 1000.
The total number of nodes is at most 5000.

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        if (!root) return { };
        vector<vector<int>> res;
        queue<Node*> q;
        q.push(root);
        while (!q.empty()){
            int size = q.size();
            vector<int> vLevel;
            for (int i = 0; i < size; i++){
                Node* curr = q.front(); q.pop();
                vLevel.push_back(curr->val);
                for (Node* child : curr->children){
                    q.push(child);
                }
            }
            res.push_back(vLevel);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    void level(Node* root, int hd, map<int, vector<int> > &m){
        if(root==NULL)return;
        
        m[hd].push_back(root->val);
        for(auto p : root->children)
            level(p, 1 + hd, m);
    }
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int> > v;
        map<int, vector<int> > m;
        level(root, 0, m);
        for(auto p : m)
        {
            vector<int> a;
            for(auto i : p.second)
                a.push_back(i);
            v.push_back(a);
        }
        return v;
    }
};
```
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        vector<vector<int>> res;
        if(!root) return res;
        m_q.push(root);
        int breadth = 1;
        while(!m_q.empty()){
            vector<int> vec;
            for(int i = 0; i < breadth; ++i){
                Node* temp = m_q.front();
                for(vector<Node*>::iterator it = temp->children.begin(); it != temp->children.end(); ++it ){
                    m_q.push(*it);
                }
                vec.push_back(temp->val);
                m_q.pop();
            }
            breadth = m_q.size();
            res.push_back(vec);
        }
        return res;
    }
private:
    queue<Node*> m_q;
};
```