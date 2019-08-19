---
layout: post
title:  "589. N-ary Tree Preorder Traversal"
date: 2019-08-11 18:02:00 -0400
categories: articles
---
Given an n-ary tree, return the preorder traversal of its nodes' values.

For example, given a 3-ary tree:

Return its preorder traversal as: [1,3,5,6,2,4].

Note:
```
Recursive solution is trivial, could you do it iteratively?
```
```c++
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> answer = {};
        if (!root)
            return answer;
    
        stack<Node*> pre;
        pre.push(root);
    
        while (!pre.empty())
        {
            Node* pot = pre.top();
            pre.pop();
    
            for (auto node = pot->children.rbegin(); node != pot->children.rend(); node++)
                pre.push(*node);
    
            answer.push_back(pot->val);
        }
    
        return answer;
    }
};
```
```c++
class Solution {
public:
    
    vector<int> dfs(Node* node) {
        vector<int> ret = vector<int>();
        if (!node) {
            return ret;
        }
        stack<Node*> stack;
        stack.push(node);
        while (!stack.empty()) {
            Node* top = stack.top();
            stack.pop();
            ret.push_back(top->val);
            reverse(top->children.begin(), top->children.end());
            for (auto child : top->children) {
                stack.push(child);
            }
        }
        return ret;
    }
    
    vector<int> preorder(Node* root) {
        return dfs(root);
    }
};
```

```c++
class Solution {
public:
    
    vector<int> preorder(Node* root) {
        vector<int> res;
        if (!root) return res;
        
        stack<Node*> s;
        s.push(root);
        while (!s.empty()){ // this is level order!!!
            Node* curr = s.top(); s.pop();
            res.push_back(curr->val);
            for (auto i = (int)curr->children.size()-1; i >= 0; i--){
                if (curr->children[i] != nullptr) {  
                    s.push(curr->children[i]);
                }
            }
        }
       
        return res;
    }
};
```
```c++
class Solution {
public:
    
    vector<int> preorder(Node* root) {
        vector<int> res;
        if (!root) return res;
        
        stack<Node*> s;
        s.push(root);
        while (!s.empty()){ // this is level order!!!
            Node* curr = s.top(); s.pop();
            res.push_back(curr->val);
            for (int i = curr->children.size()-1; i >= 0; i--){
                if (curr->children[i] != nullptr) {  
                    s.push(curr->children[i]);
                }
            }
        }
       
        return res;
    }
};

```
```c++
class Solution {
public:
    vector<int> preorder(Node* root) {
        vector<int> ret;
        
        if (!root) {
            return ret;
        }
        
        if (root->children.size() == 0) {
            ret.push_back(root->val);
            return ret;
        }
        
        ret.push_back(root->val);
        
        for (vector<Node*>::iterator it = root->children.begin(); it != root->children.end(); it++) {
            vector<int> tmp = preorder(*it);
            ret.insert(ret.end(), tmp.begin(), tmp.end());
        }
        
        return ret;
    }
};
```
```c++
class Solution {
public:
    vector<int> v;
    void preorder1(Node* root){
        if(root==NULL)
            return;
        v.push_back(root->val);
        int l=root->children.size();
        for(int i=0;i<l;++i)
        {
            preorder1(root->children[i]);
        }
    }
    vector<int> preorder(Node* root) {
        preorder1(root);
        return v;
    }
};
```