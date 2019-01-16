---
layout: post
title:  "102. Binary Tree Level Order Traversal"
date: 2019-01-12 14:56:23 -0400
categories: articles
---
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).

For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:
```
[
  [3],
  [9,20],
  [15,7]
]
```
# Function signature
```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        
    }
};
```
# 题意
给一个二叉树，返回它们的按层遍历的结果。比如从左到右，一层一层的返回。
# 想法
依然，二叉树都是从root开始的。那么想一层一层的保存起来。怎么做？？
preorder？inorder？postorder？？没有办法。但不一定不行。可以每进一层，又个标记，把这层的层数标记好。然后把这个点放在那个属于它的层的res里面。应该也是可以的。来试试。
那就preorder呗，因为感觉上就是从上往下慢慢来的。
preorder，这些遍历最方便的方法就是recursion，所以那就用recursion先来走一个。
# 尝试解解
```c++
// Accepted!
class Solution {
public:
	void helper(TreeNode* root, vector<vector<int>>& res, int level){
		if (!root) return;
		if (res.size() < level){
            vector<int> temp;
            temp.push_back(root->val);
			res.push_back(temp);
		}
		else{
			res[level-1].push_back(root->val);
		}
		helper(root->left, res, level+1);
		helper(root->right, res, level + 1);
	}
    vector<vector<int>> levelOrder(TreeNode* root) {
    	vector<vector<int>> res;
    	if (!root) return res;
    	helper(root, res, 1);
    	return res;
    }
};
```
哎，好像这样是可以的，但是。还有没有别的方法？？
