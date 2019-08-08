---
layout: post
title:  "513. Find Bottom Left Tree Value"
date: 2019-08-07 20:07:00 -0400
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
```c++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        queue<TreeNode*> q;
        q.push(root);
        
        while(!q.empty()) {
            root=q.front(); // get the value before pop coz pop will not return anyy val
            q.pop();
            if (root->right) {
                q.push(root->right);
            }
            if(root->left) q.push(root->left);
        }
        return root->val;
        
    }
};
```
```c++
class Solution {
public:
    int findBottomLeftValue(TreeNode* root) {
        deque<TreeNode*> q;
        int result = root->val;
        
        TreeNode* last_visited;
        TreeNode* cur;
        
        q.push_back(root);
        q.push_back(nullptr);
        
        while(!q.empty()) {
            cur = q.front();
            q.pop_front();
            
            if (cur == nullptr) {
                if (last_visited == nullptr) break;
                q.push_back(nullptr);
            } 
            else {
                if (last_visited == nullptr) result = cur->val;
                if (cur->left) q.push_back(cur->left);
                if (cur->right) q.push_back(cur->right);
            }
            
            last_visited = cur;
        }
        return result;
    }
};
```
```c++
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