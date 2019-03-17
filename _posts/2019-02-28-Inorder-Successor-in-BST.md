---
layout: post
title:  "285. Inorder Successor in BST"
date: 2019-02-28 21:26:23 -0400
categories: articles
---
Given a binary search tree and a node in it, find the in-order successor of that node in the BST.

The successor of a node p is the node with the smallest key greater than p.val.
Example 1:
```
Input: root = [2,1,3], p = 1
Output: 2
Explanation: 1's in-order successor node is 2. Note that both p and the return value is of TreeNode type.
```
Example 2:
```
Input: root = [5,3,6,2,4,null,null,1], p = 6
Output: null
Explanation: There is no in-order successor of the current node, so the answer is null.
```
# Shame answer
```c++
class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        if (!root ) return root;
        if ( root->val <= p->val ) 
            return inorderSuccessor(root->right, p);
        TreeNode* left = inorderSuccessor(root->left, p);
        return left == NULL ? root : left;
    }
};
```
```c++
// Accepted!!
class Solution {
public:
    TreeNode* getLeft(TreeNode* root){
        if (!root) return root;
        if (root->left) return getLeft(root->left);
        else return root;
    }
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        if ( !root ) return root;
        TreeNode* res;
        if ( p->val < root->val) {
            res = inorderSuccessor(root->left, p);
            if (!res) res = root;
        }
        else if ( p->val > root->val) res = inorderSuccessor(root->right, p);
        else res = getLeft(root->right);
        return res;
    }
};

class Solution {
public:
    TreeNode* inorderSuccessor(TreeNode* root, TreeNode* p) {
        if (!root) return root;
        if (p->val < root->val) {
            TreeNode* res = inorderSuccessor(root->left, p);
            return !res ? root : res;
            }
        else
            return inorderSuccessor(root->right, p);
    }
};

```