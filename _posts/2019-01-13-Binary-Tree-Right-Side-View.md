---
layout: post
title:  "199. Binary Tree Right Side View"
date: 2019-01-13 13:32:23 -0400
categories: articles
---
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.

Example:
```
Input: [1,2,3,null,5,null,4]
Output: [1, 3, 4]
Explanation:

   1            <---
 /   \
2     3         <---
 \     \
  5     4       <---
```
# Function signature
```c++
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        
    }
};
```
# 题意
打印这个树的右视图。
# 想法
这个题就有意思了，最方便的做法就是一层一层的找。用queue，那就是每层最后一个放进res里就行了。
# 尝试解解
```c++
// Accepted!!
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
    vector<int> res;
    if (!root) return res;
    queue<TreeNode*> myQueue;
    myQueue.push(root);
    myQueue.push(NULL);
    while( !myQueue.empty() ){
    	TreeNode* first = myQueue.front();
    	if ( first == NULL ){
    		myQueue.pop();
    		continue;
    	}
    	myQueue.pop();
    	if ( first->left )  myQueue.push(first->left);
    	if ( first->right ) myQueue.push(first->right);
    	if ( myQueue.front() == NULL) {
    		res.push_back(first->val);
    		myQueue.push(NULL);
    	}
    }
    return res;
    }
};
```
# 参考答案
```c++
class Solution {
public:
	void helper(TreeNode* root, int level, vector<int>& res){
		if (!root) return;

		if (res.size() < level){
			res.push_back(root->val);
		}
		else
			res[level-1] = root->val;

		helper(root->left,  level+1, res);
		helper(root->right, level+1, res);
	}
    vector<int> rightSideView(TreeNode* root) {
        vector<int> res;
        if (!root) return res;
        helper(root, 1, res);
        return res;
    }
};
```