---
layout: post
title:  "700. Search in a Binary Search Tree"
date: 2019-08-18 18:30:00 -0400
categories: articles
---
Given the root node of a binary search tree (BST) and a value. You need to find the node in the BST that the node's value equals the given value. Return the subtree rooted with that node. If such node doesn't exist, you should return NULL.

For example, 
```
Given the tree:
        4
       / \
      2   7
     / \
    1   3

And the value to search: 2
```
You should return this subtree:
```
      2     
     / \   
    1   3
In the example above, if we want to search the value 5, since there is no node with value 5, we should return NULL.
```
Note that an empty tree is represented by NULL, therefore you would see the expected output (serialized tree format) as [], not null.
```c++
class Solution {
public:
    //iterative
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root) {
            if(root->val == val) return root;
            else if(root->val > val) root = root->left;
            else root = root->right;
        }
        return NULL;
    }
```
```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int &val) 
    {
        if(!root)
            return NULL;
        if(root->val==val)
            return root;
        if(root->val>val)
            return searchBST(root->left,val);
        return searchBST(root->right,val);
    }
};
```
```c++
class Solution {
public:
    TreeNode* searchBST(TreeNode* root, int val) {
        if(root){
        if(root->val == val)
            return root;
        else if(root->val > val)
            return searchBST(root->left, val);
        else
            return searchBST(root->right, val);
    }
    return NULL;
    }
};
```
```c++
class Solution {
public:
    //iterative
    TreeNode* searchBST(TreeNode* root, int val) {
        while(root) {
            if(root->val == val) return root;
            else if(root->val > val) root = root->left;
            else root = root->right;
        }
        return NULL;
    }
```
```c++
class Solution {
public:
TreeNode* searchBST(TreeNode* root, int val) {
        if(root == NULL)
            return 0;
        // TreeNode * node = root;
        if(root->val < val )
            return searchBST(root->right,val);
        else if(root->val > val)
            return searchBST(root->left,val);
        else if(root->val == val )
            return root;
        
        return NULL;
        
    }
};
```