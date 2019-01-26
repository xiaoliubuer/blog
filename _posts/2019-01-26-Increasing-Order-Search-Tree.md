---
layout: post
title:  ". 897. Increasing Order Search Tree"
date: 2019-01-26 16:29:23 -0400
categories: articles
---
Given a tree, rearrange the tree in in-order so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only 1 right child.

Example 1:
```
Input: [5,3,6,2,4,null,8,1,null,null,null,7,9]

       5
      / \
    3    6
   / \    \
  2   4    8
 /        / \ 
1        7   9

Output: [1,null,2,null,3,null,4,null,5,null,6,null,7,null,8,null,9]

 1
  \
   2
    \
     3
      \
       4
        \
         5
          \
           6
            \
             7
              \
               8
                \
                 9  
Note:

The number of nodes in the given tree will be between 1 and 100.
Each node will have a unique integer value from 0 to 1000.
```
# Function signature
```c++
class Solution {
public:
    TreeNode* increasingBST(TreeNode* root) {
        
    }
};
```
# 题意
就是把头呀尾呀的换一换

# 想法
那就是inorder进去，从左脚换到右脚呗。

# 尝试解解
```c++
// Accepted！！！
class Solution {
public:
    TreeNode* helper(TreeNode* root){
        if (!root->left && !root->right) return root;
        TreeNode* left;
        if ( root->left ){
            left = increasingBST(root->left);
            TreeNode* temp = left;
            while (temp->right) {
                temp = temp->right;
            }
            temp->right = root;
            root->left = nullptr;
        }
        else
            left = root;
        root->right = increasingBST(root->right);
        return left;
    }
    TreeNode* increasingBST(TreeNode* root) {
        if (!root) return root;
        return helper(root);
    }
};
```