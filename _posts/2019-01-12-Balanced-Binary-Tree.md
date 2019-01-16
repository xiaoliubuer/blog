---
layout: post
title:  "110. Balanced Binary Tree"
date: 2019-01-12 17:31:23 -0400
categories: articles
---
Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as:

a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

Example 1:

Given the following tree [3,9,20,null,null,15,7]:
```
    3
   / \
  9  20
    /  \
   15   7
Return true.
```
Example 2:
```
Given the following tree [1,2,2,3,3,null,null,4,4]:

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
Return false.
```
# Function signature
```c++
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        
    }
};
```
# 题意
给一个二叉树，判断是不是一棵高度平衡的树。就是两个子树之间的高度差最多不超过1.
# 想法
那就遍历，算高度，如果差大于1就return false，小于1就return true。因为是比较两棵子树，那么就用postorder。返回的是每个子树的高度，如果差大于1就返回false。
# 尝试解解
```c++
class Solution {
public:
	int helper(TreeNode* root, int h){
		if (!root) return h;
		else{
			int l = helper(root->left, h+1);
			int r = helper(root->right, h+1);
		}
		if ( l = -1 || r == -1 || abs(l - r) > 1) return -1;
	}

    bool isBalanced(TreeNode* root) {
    	int height = helper(root, 1);
    	return height == -1 ? false : true;
    }
};
```
```c++
//试用递归
class Solution {
public:
	int helper(TreeNode* root){
		if (!root) return 0;
		return max(helper(root->left), helper(root->right)) + 1;
	}
    bool isBalanced(TreeNode* root) {
    	if (!root) return true;
    	// 这个地方的思路非常有意思！！！保证左边成立，保证右边成立，以及左右的差也成立！
    	return isBalanced(root->left) && isBalanced(root->right) && abs(helper(root->left)-helper(root->right)) <= 1;
    }
};
```

# 参考答案
```c++
class Solution {
public:
	int helper(TreeNode* root){
		int l, r;
        if (!root) return 0;
		l = helper(root->left);
		r = helper(root->right);
		if ( l == -1 || r == -1 || abs(l - r) > 1) return -1;
        return max(l, r)+1;
	}
    bool isBalanced(TreeNode* root) {
    	int height = helper(root);
    	return height == -1 ? false : true;
    }
};
```
```c++
class Solution {
    public:
        int height(TreeNode *root) {
            if(root == NULL)return 0;
            return max(height(root->left), height(root->right)) + 1;
        }
        bool isBalanced(TreeNode* root) {
            if(root == NULL)return true;
            return isBalanced(root->left) && isBalanced(root->right) && abs(height(root->left) - height(root->right)) <= 1;
        }
};
```