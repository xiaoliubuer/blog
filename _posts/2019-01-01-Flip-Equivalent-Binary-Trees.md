---
layout: post
title:  "951. Flip Equivalent Binary Trees"
date: 2019-01-01 08:03:23 -0400
categories: articles
---
For a binary tree T, we can define a flip operation as follows: choose any node, and swap the left and right child subtrees.

A binary tree X is flip equivalent to a binary tree Y if and only if we can make X equal to Y after some number of flip operations.

Write a function that determines whether two binary trees are flip equivalent.  The trees are given by root nodes root1 and root2.

 

Example 1:
```
Input: root1 = [1,2,3,4,5,6,null,null,null,7,8], root2 = [1,3,2,null,6,4,5,null,null,null,null,8,7]
Output: true
Explanation: We flipped at nodes with values 1, 3, and 5.
Flipped Trees Diagram
```
<!-- ![tree_ex]({{site.url}}/_img/tree_ex.jpg) -->

Note:
Each tree will have at most 100 nodes.
Each value in each tree will be a unique integer in the range [0, 99].

# 题意
对于一个二叉树，定义一个flip操作，选任意点，翻转left和right的子树。一个二叉树X翻转后等于二叉树Y,同时只有经过一些数字的翻转才能相等。

有一个flip funciton
```c++
void flip(TreeNode* root){
	TreeNode* temp = root -> left;
	root -> left = root -> right;
	root -> right= root -> temp;
}
```
# Funciton signature
```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
        
    }
};
```
# 思路
从root开始，比较left和right，如果想到，就到所有的left，如果左右不等，就flip左右。在判断是否相等。如果还是不相等，就return false，否则可以进入left，然后进入right

# 参考答案
```c++
// 卧槽，竟然一次accepted!!!
class Solution {
public:
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
    	if ( !root1 ) return root2 == nullptr;
    	if ( !root2 ) return root1 == nullptr;

    	if ( root1 -> val != root2 -> val ) return false;
    	else {
    		if ((flipEquiv(root1->left, root2->left) && flipEquiv(root1->right, root2->right)) || (flipEquiv(root1->left, root2->right) && flipEquiv(root1->right, root2->left)))
    			return true;
    		else
    			return false;
    	}
    }
};
```
```c++
// 优化后的代码
class Solution {
public:
    bool flipEquiv(TreeNode* root1, TreeNode* root2) {
    	if ( !root1 ) return root2 == nullptr;
    	if ( !root2 ) return root1 == nullptr;
    	if ( root1 -> val != root2 -> val ) return false;
    	return ((flipEquiv(root1->left, root2->left) && flipEquiv(root1->right, root2->right)) || (flipEquiv(root1->left, root2->right) && flipEquiv(root1->right, root2->left)));
    }
};
```