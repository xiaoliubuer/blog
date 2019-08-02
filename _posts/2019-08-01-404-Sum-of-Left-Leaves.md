---
layout: post
title:  "404. Sum of Left Leaves"
date: 2019-08-01 20:17:00 -0400
categories: articles
---
Find the sum of all left leaves in a given binary tree.
```
Example:

    3
   / \
  9  20
    /  \
   15   7
```
There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.

```c++
class Solution {
public:
    int sumOfLeftLeaves(TreeNode * root){
        return sumOfLeftLeaves(root, false);
    }
    int sumOfLeftLeaves(TreeNode* root, bool isLeft) {
        if (root == NULL)
            return 0;
        if (root->left == NULL && root->right == NULL){
            if (isLeft)
                return root->val;
            return 0;
        }
        return sumOfLeftLeaves(root->left, true) + sumOfLeftLeaves(root->right, false);
    }
};
```
```c++
class Solution {
    int sumOfLeftLeaves(TreeNode* node, bool isLeft){
        if (node == NULL)
            return 0;
        
        if (node->left == NULL && node->right == NULL){
            if (isLeft)
                return node->val;
            else
                return 0;
        }
        return sumOfLeftLeaves(node->left, true) + sumOfLeftLeaves(node->right, false);
    }
    
public:
    int sumOfLeftLeaves(TreeNode* root) {
        return sumOfLeftLeaves(root, false);
    }
};
```
```c++
class Solution {
public:
    int helper(TreeNode* node){
        if (node == NULL){
        	return 0 ;
        }
        if ( node->left==NULL && node->right==NULL){
        	return node->val;
        }
        int sum = 0;
        if(node->left && node->left->left == NULL && node->left->right == NULL)
        	sum += node->left->val;
       	else
       		sum += sumOfLeftLeaves(node->left);

       	sum += sumOfLeftLeaves(node->right);

        return sum;
    }
    
    int sumOfLeftLeaves(TreeNode* root) {
        if (root==NULL)
        	return 0 ;
        if(root->left == NULL && root->right == NULL) 
        	return 0;
        
        int res = helper(root);
        return res;

    }
};
```