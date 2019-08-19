---
layout: post
title:  "669. Trim a Binary Search Tree"
date: 2019-08-18 16:42:00 -0400
categories: articles
---
Given a binary search tree and the lowest and highest boundaries as L and R, trim the tree so that all its elements lies in L, R  ( R  >= L ). You might need to change the root of the tree, so the result should return the new root of the trimmed binary search tree.

Example 1:
```
Input: 
    1
   / \
  0   2

  L = 1
  R = 2

Output: 
    1
      \
       2
```
Example 2:
```
Input: 
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

Output: 
      3
     / 
   2   
  /
 1
```
```c++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if (root == NULL) return NULL;
        if (root->val < L) return trimBST(root->right, L, R);
        else if (root->val > R) return trimBST(root->left, L, R);
        
        root->left = trimBST(root->left, L, R);
        root->right = trimBST(root->right, L, R);
        return root;
    }
};
```
```c++
class Solution {
public:  
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if (root == nullptr)
            return root;
        
        if (root->val < L)
            return trimBST(root->right, L, R);
        if (root->val > R)
            return trimBST(root->left, L, R);
        
        root->left  = trimBST(root->left, L, R);
        root->right = trimBST(root->right, L, R);
        
        return root;
    }
};
```
```c++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if(!root) return root;
        if(root->left) root->left = trimBST(root->left, L, R);
        if(root->right) root->right = trimBST(root->right, L, R);
        
        // check
        if(root->val < L || root->val > R)
        {
            // here does not specify should connect left tree or right tree
            if(root->left) root = root->left;
            else if(root->right) root = root->right;
            else if(!root->right && !root->left) root = nullptr; // leaf, trim it
        }
        return root;
    }
};
```
```c++
class Solution {
public:
    TreeNode* trimBST(TreeNode* root, int L, int R) {
        if (!root)
            return root;
        if (root->val <L)
        {
            return trimBST(root->right, L, R);
        }
        else if (root->val >R)
            return trimBST(root->left, L, R);
        else{
            root->left = trimBST(root->left,L,R);
            root->right = trimBST(root->right, L, R);
        }
        return root;
    }
};
```