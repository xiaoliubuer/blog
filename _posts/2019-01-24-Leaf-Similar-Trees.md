---
layout: post
title:  "872. Leaf-Similar Trees"
date: 2019-01-24 21:42:23 -0400
categories: articles
---
Consider all the leaves of a binary tree.  From left to right order, the values of those leaves form a leaf value sequence.



For example, in the given tree above, the leaf value sequence is (6, 7, 4, 9, 8).

Two binary trees are considered leaf-similar if their leaf value sequence is the same.

Return true if and only if the two given trees with head nodes root1 and root2 are leaf-similar.

 

Note:

Both of the given trees will have between 1 and 100 nodes.
# Function signature
```c++
class Solution {
public:
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        
    }
};
```
# 题意
如果两棵树的叶子以从左往右的顺序排序，数字都相同的话，就返回true。
# 想法
不一定是什么呀的遍历，只要找出叶子，然在vector中，然后比较两个vector。
# 尝试解解
```c++
// Accepted!! 好烂的方法。
class Solution {
public:
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
    	if ( !root1 || !root2 ) return false;
    	vector<int> buff1, buff2;
    	helper(root1, buff1);
    	helper(root2, buff2);
        return buff1 == buff2;
    }

    void helper(TreeNode* root, vector<int>& buff){
    	if ( !root ) return;
    	if ( !root->left && !root->right) buff.push_back( root->val );
    	helper(root->left, buff);
    	helper(root->right,buff);
    }
};
```
```c++
//Better solution
class Solution {
public:
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
    	if ( !root1 || !root2 ) return false;
    	return helper(root1) == helper(root2);
    }
    string helper(TreeNode* root){
    	if ( !root ) return "";
    	if ( !root->left && !root->right) return to_string(root->val);
        return helper(root->left) + helper(root->right);
    }
};
```
