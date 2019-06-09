---
layout: post
title:  "103. Binary Tree Zigzag Level Order Traversal"
date: 2019-06-04 21:54:00 -0400
categories: articles
---

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).

For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its zigzag level order traversal as:
```
[
  [3],
  [20,9],
  [15,7]
]
```
```c++
/*
	TC: 
	SC: 
*/
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
		vector<vector<int>> res;
		if (!root) return res;
		unordered_map<int, int> mymap; // key: level, value: numbers
		queue<TreeNode*> myqueue;

		int level = 1;
		mymap[level] = 1;
		myqueue.push(root);
		while( !myqueue.empty() ){
			vector<int> temp;
			for (int i = 0; i < mymap[level]; ++i){
				TreeNode* cur = myqueue.front();
				myqueue.pop();
				temp.push_back(cur->val);

				if ( cur->left ){
					myqueue.push(cur->left);
					mymap[level+1]++;
				}

				if ( cur->right ){
					myqueue.push(cur->right);
					mymap[level+1]++;
				}
			}

			if (level % 2 == 0){
				reverse(temp.begin(), temp.end());
			}
			res.push_back(temp);
			level++;
		}
		return res;
    }
};
```