---
layout: post
title:  "94. Binary Tree Inorder Traversal"
date: 2019-02-27 22:44:23 -0400
categories: articles
---
Given a binary tree, return the inorder traversal of its nodes' values.

Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,3,2]
```
# My answer
```c++
class Solution {
public:
    void helper(TreeNode* root, vector<int>& res){
        if (!root) return;
        helper(root->left, res);
        res.push_back(root->val);
        helper(root->right,res);
    }
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        if (!root) return res;
        helper(root, res);
        return res;
    }
};
```