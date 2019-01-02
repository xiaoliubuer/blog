---
layout: post
title:  "BST Find Predecessor and Successor"
date: 2018-10-10 23:57:23 -0400
categories: articles
---
基础练习， 在BST里面，找一个节点Predecessor和它的Successor
```c++
// Find a node's Predecessor
TreeNode* getPredecessor(TreeNode* root){
	if (!root->right || !root)
		return root;
	else{
		while(root->right)
		root = root->right;	
	}
	return root;
}

```
To get root's Predecessor, put root->left into the function.
```c++
class Solution {
public:
    TreeNode* firstNode = nullptr;
    TreeNode* secondNode= nullptr;
    TreeNode* helper(TreeNode* root){
        if (!root) return nullptr;
        TreeNode* left = helper(root->left);
        if (left && left->val > root->val)
            if (firstNode == nullptr)
                firstNode = left;
            else
                secondNode = left;
        
        TreeNode* right = helper(root->right);
        return right == nullptr ? root : right; 
    }
    void recoverTree(TreeNode* root) {
        if (!root) return;
        helper(root);
        swap(firstNode->val, secondNode->val);
    }
};
```