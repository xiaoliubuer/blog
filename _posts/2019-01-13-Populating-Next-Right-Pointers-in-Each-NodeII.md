---
layout: post
title:  "117. Populating Next Right Pointers in Each Node II"
date: 2019-01-13 10:27:23 -0400
categories: articles
---
Given a binary tree
```
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:

You may only use constant extra space.
Recursive approach is fine, implicit stack space does not count as extra space for this problem.
Example:

Given the following binary tree,
```
     1
   /  \
  2    3
 / \    \
4   5    7
```
After calling your function, the tree should look like:
```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \    \
4-> 5 -> 7 -> NULL
```
# Function signature
```c++
class Solution {
public:
    void connect(TreeLinkNode *root) {
        
    }
};
```
# 题意
就是上道题是一颗完全树，这不一定

# 尝试解解
```c++
// 到底是哪错啦？？
class Solution {
public:
    TreeLinkNode* helper(TreeLinkNode* parent, TreeLinkNode* root){
        if (root->left && parent != root->left) 
            return root->left;
        else if (root->right && parent != root->right) 
            return root->right;
        else if (root->next) 
            return helper(parent, root->next);
        else 
            return nullptr;
    }
    void connect(TreeLinkNode *root) {
        if (!root) return;
        if (root->left){
            root->left->next  = helper(root->left, root);
        }
        if (root->right){
            root->right->next = helper(root->right, root);
        }
        connect(root->left);
        connect(root->right);
    }
};
```

# 参考答案
```c++
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if(!root || (!root->left && !root->right)) return;
        TreeLinkNode *tempRoot = root, *tempIter = new TreeLinkNode(-1);
        while(tempRoot && !tempIter->next) { 
            if(tempRoot->left) 
            	tempIter = tempIter->next = tempRoot->left;
            if(tempRoot->right) 
            	tempIter = tempIter->next = tempRoot->right;
            tempRoot = tempRoot->next;
        }
        connect(root->left); 
        connect(root->right);
    }
};
```