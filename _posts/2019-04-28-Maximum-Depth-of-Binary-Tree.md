---
layout: post
title:  "104. Maximum Depth of Binary Tree"
date: 2019-03-28 23:25:23 -0400
categories: articles
---
Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

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
return its depth = 3.


```c++
//Accepted!
class Solution {
public:
    int res = 0;
    void helper(TreeNode* root, int d) {
        if ( !root ) return;
        if ( !root->left && !root->right ) res = max(res, d);
        helper(root->left,  d + 1);
        helper(root->right, d + 1);
    }
    int maxDepth(TreeNode* root) {
        if ( !root ) return 0;
        helper(root, 1);
        return res;
    }
};
```

```c++
// Better solution
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if ( !root ) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```