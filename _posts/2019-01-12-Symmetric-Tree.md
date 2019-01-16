---
layout: post
title:  "101. Symmetric Tree"
date: 2019-01-12 14:30:23 -0400
categories: articles
---
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

For example, this binary tree [1,2,2,3,4,4,3] is symmetric:
```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```
But the following [1,2,2,null,3,null,3] is not:
```
    1
   / \
  2   2
   \   \
   3    3
```
# Function signature
```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        
    }
};
```
# 题意
就是判断一个二叉树是不是对称的

# 想法
二叉树，就是从头到尾，想要看下一个，就得有上一个。
所以一定是用递归了。那么递归的函数入口一定就是这次递归的入口指针。
用递归，需要解决的几个问题：
1. 如何退出？ if (!root) return;
2. 什么条件进入下一层？因为我们能进行对比的时候，只有两个节点在统一层，或者同在一个函数中。所以那一定是
当前这个节点的左右子树的值是相同的。时候，有机会进入下层。
3. 返回怎么呢？？在当前这层，比如：
```
    1
   / \
  2   2
 / \ / \
```
现在左右子树都相同了，那么如何判定着棵树以对称的呢？？
那是不是左子树的右节点，和右子树的左节点相同。同时左子树的左节点，和右子树的右节点相同？
但是，如果就用当前这个函数进行递归的话，是没有办法对比当前树的左子树的右节点和右子树的左节点。
所以我们需要再定义一个helper函数，用它来帮助这样的对比。先不知道返回值，那我们假设现设成void
bool helper(TreeNode* left, TreeNode* right){
	if (!left && !rignt) return true; //如果都是NULL，那就返回true
	if (left && right && left->val == right->val) {
		return helper(left->right, right->left) && helper(left->left, right->right);
	}
	else
		return false;
}
# 尝试答案
```c++
// Accepted!!
class Solution {
public:
	bool helper(TreeNode* left, TreeNode* right){
		if (!left && !right) return true; 
		if (left && right && left->val == right->val) {
			return helper(left->right, right->left) && helper(left->left, right->right);
		}
		else
			return false;
	}

    bool isSymmetric(TreeNode* root) {
    	if (!root || (!root->left && !root->right)) return true; // 这里的处理非常重要！！
    	if ( root->left && root->right && root->left->val == root->right->val)
    		return helper(root->left, root->right);
    	else
    		return false;
    }
};
```
