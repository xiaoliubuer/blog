---
layout: post
title:  "515. Find Largest Value in Each Tree Row"
date: 2019-01-26 12:30:23 -0400
categories: articles
---
You need to find the largest value in each row of a binary tree.

Example:
```
Input: 

          1
         / \
        3   2
       / \   \  
      5   3   9 

Output: [1, 3, 9]
```
# Function signature
```c++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
        
    }
};
```
# 题意
在一棵树中找到每一行的最大值。
# 想法
直接上哇！
```c++
class Solution {
public:
    vector<int> largestValues(TreeNode* root) {
    	vector<int> res;
    	if (!root) return res;
    	queue<TreeNode*> buff;
    	buff.push(root);
    	buff.push(nullptr);
    	int level = 0;
    	while( !buff.empty() ){
    		TreeNode* front = buff.front();
    		buff.pop();
    		if ( front == nullptr ){
                if ( buff.size() >= 1 ) // 对于末尾nullptr的处理，要多思考一下～
    			buff.push(nullptr);
    			level++;
    			continue;
    		}

    		if ( res.size() <= level )
    			res.push_back( front->val );
    		else
    			res[level] = max(res[level], front->val);

    		if ( front->left ) buff.push( front->left );
    		if ( front->right) buff.push( front->right);
    	}
        return res;
    }
};
```