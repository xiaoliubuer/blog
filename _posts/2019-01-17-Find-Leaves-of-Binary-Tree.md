---
layout: post
title:  "366. Find Leaves of Binary Tree"
date: 2019-01-17 21:51:23 -0400
categories: articles
---
Given a binary tree, collect a tree's nodes as if you were doing this: Collect and remove all leaves, repeat until the tree is empty.

Example:
```
Input: [1,2,3,4,5]
  
          1
         / \
        2   3
       / \     
      4   5    

Output: [[4,5,3],[2],[1]]
```
Explanation:

1. Removing the leaves [4,5,3] would result in this tree:
```
          1
         / 
        2          
``` 

2. Now removing the leaf [2] would result in this tree:
```
          1          
```

3. Now removing the leaf [1] would result in the empty tree:
```
          []         
```
# Function signature
```c++
class Solution {
public:
    vector<vector<int>> findLeaves(TreeNode* root) {
        
    }
};
```
# 题意
收集一棵树的叶子，收割一层，再割一层，直到全收完，返回每一层的叶子的vector。
# 想法
如何才能知道他是叶子呢？？左右节点都为空的时候。
那么这个时候基本就是后续遍历。发现左右都是空，然后放入vector中。 top-down， Botton-up.
就是后续遍历，helper function要返回一个层数，来决定这个点要存在那一层。
如果如果是null，就给parent层返回0，然后再往上反的就是max(left, right)。 标记着这层的node应该存到那个层里面。
# 尝试解解
```c++
// Accepted!!!
class Solution {
public:
	int helper(TreeNode* root, vector<vector<int>>& res){
		if (!root) return 0;
		int l = helper( root->left, res );
		int r = helper( root->right,res );
		int level = max(l, r);
		if ( res.size() < level + 1 )
			res.push_back(vector<int> { root->val });
		else
			res[level].push_back( root->val );
		delete root->left;
		delete root->right;
		root->left = nullptr;
		root->right= nullptr;
		return level + 1;
	}
	vector<vector<int>> findLeaves( TreeNode* root ) {
		vector<vector<int>> res;
		if (!root) return res;
		helper(root, res);
		return res;
	}
};
```