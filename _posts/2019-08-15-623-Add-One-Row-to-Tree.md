---
layout: post
title:  "623. Add One Row to Tree"
date: 2019-08-15 08:21:00 -0400
categories: articles
---	

Given the root of a binary tree, then value v and depth d, you need to add a row of nodes with value v at the given depth d. The root node is at depth 1.

The adding rule is: given a positive integer depth d, for each NOT null tree nodes N in depth d-1, create two tree nodes with value v as N's left subtree root and right subtree root. And N's original left subtree should be the left subtree of the new left subtree root, its original right subtree should be the right subtree of the new right subtree root. If depth d is 1 that means there is no depth d-1 at all, then create a tree node with value v as the new root of the whole original tree, and the original tree is the new root's left subtree.

Example 1:
```
Input: 
A binary tree as following:
       4
     /   \
    2     6
   / \   / 
  3   1 5   

v = 1

d = 2

Output: 
       4
      / \
     1   1
    /     \
   2       6
  / \     / 
 3   1   5   
```
Example 2:
```
Input: 
A binary tree as following:
      4
     /   
    2    
   / \   
  3   1    

v = 1

d = 3

Output: 
      4
     /   
    2
   / \    
  1   1
 /     \  
3       1
```
Note:
```
The given d is in range [1, maximum depth of the given tree + 1].
The given binary tree has at least one tree node.
```
```c++
class Solution {
public:
TreeNode* addOneRow(TreeNode* root, int v, int d) {
        TreeNode * n = NULL;
        if (d == 1) {
            n = new TreeNode(v);
            n->left = root;
            return n;
        } else if (d == 2 && root != NULL) {
            n = new TreeNode(v);
            n->left = root->left;
            root->left = n;
            n = new TreeNode(v);
            n->right = root->right;
            root->right = n;
            return root;
        } else if (root != NULL){
            root->left = addOneRow(root->left, v, d - 1);
            root->right = addOneRow(root->right, v, d - 1);
            return root;
        } else {
            return NULL;
        }
    }
};
```
```c++
class Solution {
public:
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        queue<TreeNode*> q;
        TreeNode* pre = new TreeNode(-1);
        pre->left = root;
        q.push(pre);
        
        for (int i = 0; i < d - 1; ++i) {
            int n = q.size();
            for (int j = 0; j < n; ++j) {
                root = q.front();
                q.pop();
                if (root->left) q.push(root->left);
                if (root->right) q.push(root->right);
            }
        }
        
        while (q.size()) {
            root = q.front();
            q.pop();
            if (root->left) {
                TreeNode* tmp = root->left;
                root->left = new TreeNode(v);
                root->left->left = tmp;
            }
            else root->left = new TreeNode(v);
            
            if (root->right) {
                TreeNode* tmp = root->right;
                root->right = new TreeNode(v);
                root->right->right = tmp;
            }
            else root->right = new TreeNode(v);
        }
        
        return pre->left;
    }
};
```
```c++
class Solution {
public:
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if(d == 1) {
            TreeNode* newRoot = new TreeNode(v);
            newRoot->left = root;
            return newRoot;
        }
        queue<pair<int, TreeNode*> > q;
        q.push(pair(1, root));
        while(!q.empty()) {
            pair<int, TreeNode*> curr = q.front(); q.pop();
            int level = curr.first; TreeNode* node = curr.second;
            if(node == NULL) continue;
            if(level == d-1) {
                addNode(node, v);
                continue;
            }
            q.push(pair(level+1, node->left)); q.push(pair(level+1, node->right));
        }
        return root;
    }
    
    void addNode(TreeNode* root, int v) {
        TreeNode* left = root->left; TreeNode* right = root->right;
        TreeNode* newL = new TreeNode(v);  TreeNode* newR = new TreeNode(v);
        root->left = newL; root->right = newR;
        newL -> left = left; newR -> right = right;
        
    }
};
```


```c++
class Solution {
public:
    TreeNode* addOneRow(TreeNode* root, int v, int d) {
        if(d == 1) {
        	TreeNode *newRoot = new TreeNode(v);
        	newRoot->left = root;
        	return newRoot;
		}
		
		pair<TreeNode*, TreeNode*> children; // {LeftChild, RightChild}
		queue<TreeNode*> q;
		q.push(root);
		int currentLevel = 1;
		while(!q.empty()) {
			int sz = q.size();
			while(sz--) {
				TreeNode* current = q.front();
				q.pop();
				if(currentLevel == d-1) {
					if(current->left) {
						children.first = current->left;	
						current->left = NULL;
						current->left = new TreeNode(v);
						current->left->left = children.first;
					}
					else current->left = new TreeNode(v);
					if(current->right) {
						children.second = current->right;	
						current->right = NULL;
						current->right  = new TreeNode(v);
						current->right->right = children.second;
					}
					else current->right  = new TreeNode(v);
				}
				if(current->left) q.push(current->left);
				if(current->right) q.push(current->right);
			}
			currentLevel++;
		}
		return root;
    }
};
```