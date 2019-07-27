---
layout: post
title:  "144. Binary Tree Preorder Traversal"
date: 2019-07-27 07:00:23 -0400
categories: articles
---

Given a binary tree, return the preorder traversal of its nodes' values.

Example:
```
Input: [1,null,2,3]
   1
    \
     2
    /
   3

Output: [1,2,3]
```
Follow up: Recursive solution is trivial, could you do it iteratively?

```c++
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> res;
        if ( !root ) return res;
        stack<TreeNode*> buffer;
        buffer.push(root);
        while ( !buffer.empty() ){
            TreeNode* top = buffer.top();
            res.push_back( top->val );
            buffer.pop();
            if ( top->right ) buffer.push( top->right );
            if ( top->left ) buffer.push( top->left );
        }
        return res;
    }
};
```