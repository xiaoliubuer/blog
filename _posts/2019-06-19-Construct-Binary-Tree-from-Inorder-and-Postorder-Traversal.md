---
layout: post
title:  "106. Construct Binary Tree from Inorder and Postorder Traversal"
date: 2019-06-19 20:42:00 -0400
categories: articles
---
Given inorder and postorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given
```
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
```
Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```
```c++
class Solution {
private:
        unordered_map<int, int> inm; // inorder map [inorder[i], i]

public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        int n = inorder.size(), i = 0;
        for(auto val: inorder) inm[val] = i++; // build inm for dfs 
        return dfs(inorder, 0, n - 1, postorder, 0, n - 1);
    }
    
    TreeNode* dfs(vector<int>& inorder, int ileft, int iright, vector<int>& postorder, int pleft, int pright) {
        if(ileft > iright) return nullptr;
        
        int val = postorder[pright]; // root value
        TreeNode *root = new TreeNode(val);
        if(ileft == iright) return root;
        
        int iroot = inm[val];
        int nleft = iroot - ileft; // length of left subtree
        root->right = dfs(inorder, iroot + 1, iright, postorder, pleft + nleft, pright - 1);
        root->left = dfs(inorder, ileft, iroot - 1, postorder, pleft, pleft + nleft - 1);
        
        return root;
    }
};
```