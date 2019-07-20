---
layout: post
title:  "105. Construct Binary Tree from Preorder and Inorder Traversal"
date: 2019-06-25 15:39:23 -0400
categories: articles
---
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.

For example, given
```
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
```
Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```
# Function signature
```
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        
    }
};
```
# 题意
通过preorder顺序和inorder顺序构建一颗二叉树。
# 想法
它说的是preorder和inorder，那这个说明啥啦？？构建树都是从根(root)节点开始构建的。那么如何找到根节点呢？？
注意两种遍历方式，preorder和inorder:
```c++
// preorder
root->val;
helper(root->left);
helper(root->right);
```
```c++
// inorder
helper(root->left);
root->val;
helper(root->right);
```
所以从preorder的顺序里，有一找到root是谁。那就是preorder的第一个元素，那就是3.
那root找到了，如何找到左子树和右子树。我们发现，在inorder的算法中，root左边的都是他的左子树，root右边的都是它的右子树。
所以在inorder的顺序里面，我们发现9就是3的左子树，[15, 20, 7]就是它的右子树。
然后，我们要知道，二叉树的无论是整体这个棵树，还是各个子树，用不同遍历方法遍历的时候，各个子树也是遵守那个顺序的。所以在各个子树中寻找它们的root，它们的左子树，它们的右子树也用也同样的道理。
所以9只是一个node，我们就知道他是3的左子树了，但是[15,20,7], 通过preorder的顺序，我们发现20应该是root，再通过inorder，发现15是左子树，7是右子树。
所以就得出了最后的答案。
那如何实现呢？？

这里如何处理是个很好的问题。
就是如何split原来的inorder，然后在下一次的循环中只用这一部分。或者传index。
如果每一次新创建vector，会带来更多的space浪费，但是如果用下标，会是过程变得非常复杂不好维护。

那怎么办呢？？卡在了如何拆分这两个vector，这个地方好复杂。
答案用的是index。我来试试。

```c++
class Solution {
public:
    unordered_map<int, int> map_idx;
    vector<int> post;
    TreeNode* helper(int& index, int left, int right){
        if(left == right) return nullptr;
        TreeNode *root = new TreeNode(post[index]);
        int pivot = map_idx[post[index]];
        index--;
        root->right = helper(index, pivot + 1, right);
        root->left = helper(index, left, pivot);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        post = postorder;
        int size = postorder.size();
        for(int i = 0; i < size; i++){
            map_idx[inorder[i]] = i;
        }
        int index = size - 1;
        return helper(index, 0, size);
    }
};
```
```c++
// preorder = [3,9,20,15,7]
// inorder  = [9,3,15,20,7]

class Solution {
public:
	TreeNode* helper(vector<int> preorder, int preL, int preR, vector<int> inorder, int inL, int inR, unordered_map<int, int>& idxMap){
        if (inL > inR) return nullptr; //以及这里的
        int value = preorder[preL];
        int idx_root = idxMap[value];
		TreeNode* root = new TreeNode(value); //

		int len = idx_root - inL; // 这个地方太巧妙
		if (inL == inR) return root; //还有这个地方的corner case

		root->left = helper(preorder, preL+1, preL + len, inorder, inL, idx_root-1, idxMap); //如果构建的话，一定是把发回来的值赋值给当前层的left or right，而不是把它们传下去，因为传下去传的是pointer的copy。
		root->right = helper(preorder, preL+len+1, preR, inorder, idx_root+1, inR, idxMap);
        return root;
	}
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
       TreeNode* root;
       unordered_map<int, int> idxMap;
       for (int i = 0; i < inorder.size(); ++i) idxMap[inorder[i]] = i;
       int m = preorder.size();
       int n = inorder.size();
       return helper(preorder, 0, m-1, inorder, 0, n-1, idxMap);
    }
};
```


