---
layout: post
title:  "543. Diameter of Binary Tree"
date: 2019-01-22 20:59:23 -0400
categories: articles
---
Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree. This path may or may not pass through the root.

Example:
```
Given a binary tree 
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```
Note: The length of path between two nodes is represented by the number of edges between them.

```c++
class Solution {
public:
    int findHeight(TreeNode* root, int &maxAns){
        if(root == NULL) return 0;
        
        int l = findHeight(root->left,maxAns);
        int r = findHeight(root->right,maxAns);
        
        maxAns = max(maxAns,l+r);
        
        return max(l,r) + 1;
    }
    
    int diameterOfBinaryTree(TreeNode* root) {
        int maxAns = 0;
        findHeight(root,maxAns);
        return maxAns;
    }
};
```
```c++
class Solution {
private:
    inline int helper(TreeNode* root,int& result){
        if(root!=nullptr){
            int left=helper(root->left,result);
            int right=helper(root->right,result);
            result=max(result,right+left);
            return max(left,right)+1;
        }else{
            return 0;
        }
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        int result=0;
        helper(root,result);
        return result;
    }
};
```
```c++
class Solution {
public:
	int max_length = 0;
	int longestLength(TreeNode* root){
		if (!root)	return 0;

		int length_left  = longestLength(root->left);
		int length_right = longestLength(root->right);

		max_length = std::max(max_length, length_left+length_right);

		return max(length_left, length_right)+1;
	}
    int diameterOfBinaryTree(TreeNode* root) {
		if ( root == NULL ){
			return 0;
		}

		int res = longestLength(root);
		return max_length;
    }
};
```

```c++
class Solution {
private:
    int diameterOfBinaryTree_(TreeNode* root, int& length){
        if (root == NULL){
            return 0;
        }
        
        int lenLeft = 0;
        int lenRight = 0;
        int ans = max(diameterOfBinaryTree_(root->left, lenLeft), diameterOfBinaryTree_(root->right, lenRight));
        
        length = max(lenLeft, lenRight) + 1;
        return max(ans, lenLeft + lenRight);
    }
public:
    int diameterOfBinaryTree(TreeNode* root) {
        if (root == NULL){
            return 0;
        }
        int t = 0;
        int ans = diameterOfBinaryTree_(root, t);
        return ans;
    }
};
```