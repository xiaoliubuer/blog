---
layout: post
title:  "250. Count Univalue Subtrees"
date: 2019-07-28 16:02:00 -0400
categories: articles
---
Given a binary tree, count the number of uni-value subtrees.
A Uni-value subtree means all nodes of the subtree have the same value.
Example :
```
Input:  root = [5,1,5,5,5,null,5]

              5
             / \
            1   5
           / \   \
          5   5   5
```
Output: 4
```c++
class Solution {
private:
    int num = 0;
public:
    int countUnivalSubtrees(TreeNode* root) {
        if(!root)
            return 0;
        dfs(root, root->val);
        return num;
    }
    
    bool dfs(TreeNode* root, int prev){
        if(!root)
            return true;
        bool flag1 = dfs(root->left, root->val);
        bool flag2 = dfs(root->right, root->val);
        if(flag1 && flag2)
            num++;
        return (prev == root->val) && flag1 && flag2;
    }
};
```