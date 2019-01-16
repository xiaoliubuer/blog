---
layout: post
title:  "114. Flatten Binary Tree to Linked List"
date: 2019-01-12 18:26:23 -0400
categories: articles
---
Given a binary tree, flatten it to a linked list in-place.

For example, given the following tree:
```
    1
   / \
  2   5
 / \   \
3   4   6
```
The flattened tree should look like:
```
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
```
# Function signature
```c++
class Solution {
public:
    void flatten(TreeNode* root) {
        
    }
};
```
# 题意
给一个二叉树，传换成一个linked list.
# 想法
判断这个试preorder？inorder？还是postorder？
仔细想一下preorder，inorder and postorder分别会在当前层做哪事事情。

如果是preorder，那么就是要把2放到right，把5连到最后面。所以处理5是需要2这边都处理完了。
2 -> right;
helper(root->left);
5 -> 2's end
helper(root->right);
is this OK?
# 尝试解解
```c++
// 卧槽，竟然过啦！！
class Solution {
public:
	TreeNode* helper(TreeNode* root){
		if(!root) return root;
		TreeNode* tempL = root->left;
		root->left = nullptr;
		TreeNode* tempR = root->right;
		root->right = tempL;
		helper(tempL);
		TreeNode* curr = root->right;
		while(curr && curr->right) curr = curr->right;
		if (curr) curr->right = tempR;
        else root->right = tempR;
		helper(tempR);
		return root;
	}
    void flatten(TreeNode* root) {
        if (!root) return;
        helper(root);
    }
};
```
# 参考答案
```c++
class Solution {
public:
    void flatten(TreeNode* root) {
        if ( !root ) return;
            TreeNode* right = root->right;
            TreeNode* cur = root->left;
        while ( cur && cur->right){
            cur = cur->right;
        }
        if( cur ){
            cur->right = right;
            root->right = root->left;
            root->left = NULL;
        }
            flatten(root->right);
    }
};
```
