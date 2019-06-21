---
layout: post
title:  "113. Path Sum II"
date: 2019-06-20 19:29:00 -0400
categories: articles
---


Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.

Note: A leaf is a node with no children.

Example:
```
Given the below binary tree and sum = 22,

      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
Return:

[
   [5,4,11,2],
   [5,8,4,5]
]
```


```c++
class Solution {
public:
    vector<vector<int>> res;
    void helper(TreeNode* root, int sum, vector<int> path){
        if (!root) return;
        path.push_back(root->val);
        if (!root->left && !root->right && sum == root->val){
            res.push_back(path);
            return;
        }
        helper(root->left,  sum - root->val, path);
        helper(root->right, sum - root->val, path);
    }
    
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<int> path;
        helper(root, sum, path);
        return res;
    }
};
```