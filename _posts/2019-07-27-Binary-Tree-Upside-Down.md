---
layout: post
title:  "156. Binary Tree Upside Down"
date: 2019-07-27 19:18:00 -0400
categories: articles
---
Given a binary tree where all the right nodes are either leaf nodes with a sibling (a left node that shares the same parent node) or empty, flip it upside down and turn it into a tree where the original right nodes turned into left leaf nodes. Return the new root.

Example:
```
Input: [1,2,3,4,5]

    1
   / \
  2   3
 / \
4   5

Output: return the root of the binary tree [4,5,2,#,#,3,1]

   4
  / \
 5   2
    / \
   3   1  
```
Clarification:

Confused what [4,5,2,#,#,3,1] means? Read more below on how binary tree is serialized on OJ.

The serialization of a binary tree follows a level order traversal, where '#' signifies a path terminator where no node exists below.

Here's an example:
```
   1
  / \
 2   3
    /
   4
    \
     5
```
The above binary tree is serialized as [1,2,3,#,#,4,#,#,5].

```c++
class Solution {
public:
    TreeNode* upsideDownBinaryTree(TreeNode* root) {
	if (!root || !root->left) return root;

	    TreeNode* cur_left = root->left;
	    TreeNode* cur_right = root->right;
	    TreeNode* new_root = upsideDownBinaryTree(root->left);

	    cur_left->right = root;
	    cur_left->left = cur_right;
	    
	    root->left = nullptr;
	    root->right = nullptr;

	    return new_root;
    }
};
```