---
layout: post
title:  "590. N-ary Tree Postorder Traversal"
date: 2019-08-11 18:04:00 -0400
categories: articles
---
Given an n-ary tree, return the postorder traversal of its nodes' values.

For example, given a 3-ary tree:
Return its postorder traversal as: [5,6,3,2,4,1].

Note:

Recursive solution is trivial, could you do it iteratively?

```c++
class Solution {
    vector<int> res;
public:
    void helper(Node* root, vector<int>& res) {
        if (root == NULL) return;
        
        for (int i = 0; i < root->children.size(); i++) {
                //cout<<a->children[i]->val<<endl;
                helper(root->children[i], res);
                res.push_back(root->children[i]->val);
        }   
    }
    vector<int> postorder(Node* root) {
        if (root == NULL) return vector<int> {};
        helper(root, res);
        res.push_back(root->val);
        return res;
    }
};
```
```c++
class Solution {
    vector<int> res;
public:
    void helper(Node* root, vector<int>& res) {
        if (root == NULL) return;
        
        for (int i = 0; i < root->children.size(); i++) {
                //cout<<a->children[i]->val<<endl;
                helper(root->children[i], res);
                res.push_back(root->children[i]->val);
        }   
    }
    vector<int> postorder(Node* root) {
        if (root == NULL) return vector<int> {};
        helper(root, res);
        res.push_back(root->val);
        return res;
    }
};
```
```c++
class Solution {
    void helper(Node* root, vector<int>& ans){
        if(root == nullptr) return;
        for(auto i:root->children){
            helper(i, ans);
        }
        ans.push_back(root->val);
    }
public:
    vector<int> postorder(Node* root) {
        vector<int> ans;
        helper(root, ans);
        return ans;
    }
};
```