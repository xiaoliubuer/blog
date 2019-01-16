---
layout: post
title:  "111. Minimum Depth of Binary Tree"
date: 2019-01-12 18:11:23 -0400
categories: articles
---
Given a binary tree, find its minimum depth.

The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.

Note: A leaf is a node with no children.

Example:

Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its minimum depth = 2.
# Function signature
```c++
class Solution {
public:
    int minDepth(TreeNode* root) {
        
    }
};
```
# 题意
给一个树找到它的最小深度。
# 想法
那就递归分别找呗，返回最小就行啦
# 尝试解解
```c++
class Solution {
public:
    int minDepth(TreeNode* root) {
    	if (!root) return 0;
        if (!root->left && !root->right) return 1;
        int l = minDepth(root->left);
        int r = minDepth(root->right);
        if (l != 0 && r != 0) return min(l,r) + 1;
        else return max(l,r) + 1;
    }
};
```
# 参考答案
```c++
class Solution {
public:
    int minDepth(TreeNode *root) {
        if (root == NULL) return 0;
        if (root->left == NULL && root->right == NULL) return 1;
        if (root->left == NULL) return minDepth(root->right) + 1;
        else if (root->right == NULL) return minDepth(root->left) + 1;
        else return 1 + min(minDepth(root->left), minDepth(root->right));
    }
    
};
```



