---
layout: post
title:  "226. Invert Binary Tree"
date: 2019-07-27 19:35:00 -0400
categories: articles
---
Invert a binary tree.

Example:
```
Input:

     4
   /   \
  2     7
 / \   / \
1   3 6   9
Output:

     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
Trivia:
This problem was inspired by this original tweet by Max Howell:

Google: 90% of our engineers use the software you wrote (Homebrew), but you can’t invert a binary tree on a whiteboard so f*** off.

```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if ( !root ) return root;
        TreeNode* temp = invertTree(root->left);
        root->left = invertTree(root->right);
        root->right = temp;
        return root;
    }
};
```