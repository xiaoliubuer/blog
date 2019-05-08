---
layout: post
title:  "98. Validate Binary Search Tree"
date: 2019-02-28 20:58:23 -0400
categories: articles
---
Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
Example 1:
```
Input:
    2
   / \
  1   3
Output: true
```
Example 2:
```
    5
   / \
  1   4
     / \
    3   6
Output: false
```
Explanation: The input is: [5,1,4,null,null,3,6]. The root node's value is 5 but its right child's value is 4.
# Function signature
```c++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        
    }
};
```
# 题意
给一个二叉树，判断是不是一个合法的BST。
# 想法
BST性质，左子树一定都小于root，右子树一定都大于根。这一条使用每个subtree。
所以root行不行，取决于它的left 和 right。 那对于每个节点，他的 left 和 right 都有一个边界。
root一定要大于left的右边界，一定小于right的左边界。

其实这里就是两种解决方法，top-down，Botton-up。参考答案用的是第一种，我用的是第二种，可惜暂时不work，有小bug。
```c++
// Interesting!!!
class Solution {
public:
    bool helper(TreeNode* lEdge, TreeNode* rEdge, TreeNode* root){
        if ( !root ) return true;
        bool left = helper(lEdge, root, root->left);
        bool right= helper(root, rEdge, root->right);
        if (lEdge && lEdge->val >= root->val ) return false;
        if (rEdge && rEdge->val <= root->val ) return false;
        return left && right;
    }
    bool isValidBST(TreeNode* root) {
        if ( !root ) return true;
        return helper(NULL, NULL, root);
    }
};
```
# 尝试解解
```c++
// 总体思路是对的，但是就是没有过～是因为还有corner case没有处理到。
class Solution {
public:
	class Node {
        public:
		Node(){};
		Node(bool status, int leftEdge, int rightEdge):status(status), leftEdge(leftEdge), rightEdge(rightEdge){};
		int leftEdge;
		int rightEdge;
		bool status;
	};
    
	Node* helper(TreeNode* root){
		if ( !root ) return new Node(true, 0,0);
		Node* left  = helper( root->left  );
		Node* right = helper( root->right );
        if (left->leftEdge == 0 && left->rightEdge == 0 && right->leftEdge == 0 && right->rightEdge == 0) return new Node(true, root->val, root->val); 
		if ( left->status && right->status && root->val >= left->rightEdge && root ->val < right->leftEdge){
			return new Node(true, left->leftEdge, right->rightEdge);
		}
		else
			return new Node(false, 0, 0);
	}

    bool isValidBST(TreeNode* root) {
    	if (!root) return true;
        return helper(root)->status;
    }
};
```

# My Wrong answer
```c++
// 即使能保证每个根节点和叶子结点符合条件，但还要保证整棵树也全部满足。所以仅仅看相邻的结点是不够的。
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        bool l = isValidBST(root->left);
        if(root->left && root->val <= root->left->val) return false;
        bool r = isValidBST(root->right);
        if(root->right && root->val >= root->right->val) return false;
        return l && r;
    }
};
```
```c++
// Accepted 02/28/2019
class Solution {
public:
    
    bool helper(TreeNode* root, TreeNode* lefEdge, TreeNode* rigEdge){
        if (!root) return true;
        bool l = helper(root->left, lefEdge, root);
        bool r = helper(root->right,root, rigEdge);
        if (lefEdge && root->val <= lefEdge->val) return false;
        if (rigEdge && root->val >= rigEdge->val) return false;
        return l && r;
    }
    
    bool isValidBST(TreeNode* root) {
        if (!root) return true;
        return helper(root, NULL, NULL);
    }
};
```

# 参考答案
```c++
class Solution {
public:
    bool helper(TreeNode* root, TreeNode* left_edge, TreeNode* right_edge){
      if (!root) return true;
      if ((left_edge && root->val <= left_edge->val) || (right_edge && root->val >= right_edge->val)){
        return false;
      }
      return helper(root->left, left_edge, root) && helper(root->right, root, right_edge);
    }

    bool isValidBST(TreeNode* root) {
    if (!root) return true;  
    return helper(root, NULL, NULL);
    }
};
```
