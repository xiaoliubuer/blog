---
layout: post
title:  "450. Delete Node in a BST"
date: 2019-08-02 20:49:00 -0400
categories: articles
---
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.

Basically, the deletion can be divided into two stages:

Search for a node to remove.
If the node is found, delete the node.
Note: Time complexity should be O(height of tree).

Example:
```
root = [5,3,6,2,4,null,7]
key = 3

    5
   / \
  3   6
 / \   \
2   4   7
````
Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.
```
    5
   / \
  4   6
 /     \
2       7
```
Another valid answer is [5,2,6,null,4,null,7].
```
    5
   / \
  2   6
   \   \
    4   7
```

```c++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return nullptr;
        if (root->val == key) {
            if (!root->right) {
                TreeNode* left = root->left;
                delete root;
                return left;
            }
            else {
                TreeNode* right = root->right;
                while (right->left)
                    right = right->left;
                swap(root->val, right->val);    
            }
        }
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
};
```
```c++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return nullptr;
        if (root->val == key) {
            if (!root->right) {
                TreeNode* left = root->left;
                delete root;
                return left;
            }
            else {
                TreeNode* right = root->right;
                while (right->left)
                    right = right->left;
                swap(root->val, right->val);    
            }
        }
        root->left = deleteNode(root->left, key);
        root->right = deleteNode(root->right, key);
        return root;
    }
};
```
```c++
class Solution {
public:
    TreeNode* deleteNode(TreeNode* root, int key) {
        if (!root) return root;
        if (root->val == key){
	      	if (!root->right){
	      		TreeNode* left = root->left;
	      		delete root;
	      		return root->left;
	      	}
	      	else{
	      		TreeNode* right = root->right;
	      		while(right->left)
	      			right = right->left;
	      			swap(root->val, right->val);
	      	}
		}
		root->left =  deleteNode(root->left, key);
		root->right = deleteNode(root->right,key);
		return root;
    }
};
```