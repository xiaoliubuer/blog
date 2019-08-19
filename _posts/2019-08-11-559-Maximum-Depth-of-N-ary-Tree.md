---
layout: post
title:  "559. Maximum Depth of N-ary Tree"
date: 2019-08-11 16:40:00 -0400
categories: articles
---
Given a n-ary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

For example, given a 3-ary tree:

We should return its max depth, which is 3.

Note:
```
The depth of the tree is at most 1000.
The total number of nodes is at most 5000.
```

```c++
class Node {
public:
    int val;
    vector<Node*> children;

    Node() {}

    Node(int _val, vector<Node*> _children) {
        val = _val;
        children = _children;
    }
};
```
```c++
class Solution {
public:
    int maxDepth(Node* root) {
        if (!root) return 0;
        int m = 0;
        for (auto i : root->children){
            m = max(m, maxDepth(i));
        }
        return m + 1;
    }
};
```
```c++
class Solution {
public:
    int maxDepth(Node* root) {
        if(root==nullptr) return 0;
        
        int height = 0;
        for(auto node : root->children)
            height = max(height,maxDepth(node));
        return 1+height;
    }
};
```
```c++
class Solution {
public:
    
    int fn(Node* root)
    {
        if(root->children.size()==0)return 1;
        int l=root->children.size();
        int Max=0;
        for(int i=0;i<l;i++)
        {
            Max=max(Max,1+fn(root->children[i]));
        }
        return Max;
    }
    int maxDepth(Node* root) {
        if(root==NULL)return 0;
        return fn(root);
    }
};
```
```c++
class Solution {
private:
    queue<Node*> myQ;
public:
    int maxDepth(Node* root) {
        if(root == NULL) return 0;
        int depth = 0;
        myQ.push(root);
        while(!myQ.empty()){
            depth ++;
            int size = myQ.size();
            for(int i = 0; i < size; i ++){
                Node* cur = myQ.front();
                myQ.pop();
                for(Node* c: cur->children) myQ.push(c);
            }   
        }
        return depth;
    }
};
```
```c++
class Solution {
public:
    int maxDepth(Node* root) {
        if(root == NULL) return 0;
        int maxDep = 0;
        for(auto c: root->children) maxDep = max(maxDep,maxDepth(c));
        return maxDep + 1;
    }
};
```
```c++
class Solution {
public:
    int maxDepth(Node* root) {
        if (!root) return 0;
        int maxVal = 0;
        
        for (auto& c: root->children) {
            maxVal = max(maxVal, maxDepth(c));
        }
        
        return maxVal + 1;
    }
};
```