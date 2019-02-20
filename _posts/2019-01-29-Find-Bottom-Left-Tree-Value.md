---
layout: post
title:  "513. Find Bottom Left Tree Value"
date: 2019-01-29 22:29:23 -0400
categories: articles
---
Given a binary tree, find the leftmost value in the last row of the tree.

Example 1:
```
Input:

    2
   / \
  1   3

Output:
1
```
Example 2: 
```
Input:

        1
       / \
      2   3
     /   / \
    4   5   6
       /
      7

Output:
7
```
Note: You may assume the tree (i.e., the given root node) is not NULL.
# Function signature
```c++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        
    }
};
```
# 题意

# 尝试解解
```c++
// Accepted !!
class Solution {
public:
    int res;
    int deep = -1;
    void helper(TreeNode* root, int level){
        if ( !root ) return;
        if ( !root->left && !root->right && level > deep ) {
            res = root->val;
            deep = level;
        }
        helper( root->left, level + 1 );
        helper( root->right,level + 1 );
    }
    int findBottomLeftValue(TreeNode* root) {
        helper(root, 0);
        return res;
    }
};
```