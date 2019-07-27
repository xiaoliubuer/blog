---
layout: post
title:  "107. Binary Tree Level Order Traversal II"
date: 2019-06-04 21:58:00 -0400
categories: articles
---

Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).

For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its bottom-up level order traversal as:
```
[
  [15,7],
  [9,20],
  [3]
]
```
```c++
class Solution {
  int maxDepth(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth(root->left), maxDepth(root->right));
  }
  void run(TreeNode* root, int level, vector<vector<int>>& res) {
    if (!root) return;
    res[res.size() - level - 1].push_back(root->val);
    run(root->left, level + 1, res);
    run(root->right, level + 1, res);
  }  
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
      vector<vector<int>> res;
      res.resize(maxDepth(root));
      run(root, 0, res);
      return res;
    }
};
```
```c++
class Solution {
public:

	void helper(TreeNode* root, int level, vector<vector<int>>& res, int height){
		if (!root->left && !root->right) return;
        int hl = 0, hr = 0;
        if (root->left)  helper(root->left, level+1, res, height); 
        if (root->right) helper(root->right, level+1,res, height);

		if (res.size() < height - level){
            while(res.size() < height-level){
                vector<int> temp;
                res.push_back(temp);
            }
		}
		if (root->left)
			res[height - level - 1].push_back(root->left->val);
		if (root->right)
			res[height - level - 1].push_back(root->right->val);
	}
    
    int max_height(TreeNode* root){
        if (!root) return 0;
        int height = max(max_height(root->left),max_height(root->right)) + 1;
        return height;
    };

    vector<vector<int>> levelOrderBottom(TreeNode* root) {
        vector<vector<int>> res;
        if (!root) return res;
        int height = max_height(root);
        helper(root, 1, res, height);
        vector<int> temp(1, root->val);
        res.push_back(temp);
        return res;
    }
};
```