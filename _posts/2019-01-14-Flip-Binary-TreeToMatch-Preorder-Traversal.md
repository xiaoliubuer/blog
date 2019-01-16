---
layout: post
title:  "** 971. Flip Binary Tree To Match Preorder Traversal"
date: 2019-01-14 21:38:23 -0400
categories: articles
---
Given a binary tree with N nodes, each node has a different value from {1, ..., N}.

A node in this binary tree can be flipped by swapping the left child and the right child of that node.

Consider the sequence of N values reported by a preorder traversal starting from the root.  Call such a sequence of N values the voyage of the tree.

(Recall that a preorder traversal of a node means we report the current node's value, then preorder-traverse the left child, then preorder-traverse the right child.)

Our goal is to flip the least number of nodes in the tree so that the voyage of the tree matches the voyage we are given.

If we can do so, then return a list of the values of all nodes flipped.  You may return the answer in any order.

If we cannot do so, then return the list [-1].

 

Example 1:
```
Input: root = [1,2], voyage = [2,1]
Output: [-1]
```
Example 2:
```
Input: root = [1,2,3], voyage = [1,3,2]
Output: [1]
```
Example 3:
```
Input: root = [1,2,3], voyage = [1,2,3]
Output: []
```
Note:
```
1 <= N <= 100
```
# Function signature
```c++
class Solution {
public:
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& voyage) {
        
    }
};
```
# 题意
一个二叉树和一个voyage，找到最小的方式可以flip left with right使得二叉树复合voyage。
# 想法
没啥想法这道题
# Shame answer
```c++
    vector<int> res;
    int i = 0;
    vector<int> flipMatchVoyage(TreeNode* root, vector<int>& v) {
        return dfs(root, v) ? res : vector<int>{-1}; // 这个地方 vector<int>{-1}
    }

    bool dfs(TreeNode* node, vector<int>& v) {
        if (!node) return true;
        if (node->val != v[i++]) return false; // 这个技巧太厉害啦 i++
        if (node->left && node->left->val != v[i]) {
            res.push_back(node->val);
            return dfs(node->right, v) && dfs(node->left, v); // 巧妙！非常巧妙，对于递归的使用真的已经是炉火纯青了！
        }
        return dfs(node->left, v) && dfs(node->right, v);
    }
```