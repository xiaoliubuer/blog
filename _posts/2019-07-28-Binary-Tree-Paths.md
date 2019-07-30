---
layout: post
title:  "257. Binary Tree Paths"
date: 2019-07-28 16:13:00 -0400
categories: articles
---
Given a binary tree, return all root-to-leaf paths.
Note: A leaf is a node with no children.
Example:
```
Input:

   1
 /   \
2     3
 \
  5

Output: ["1->2->5", "1->3"]
```
Explanation: All root-to-leaf paths are: 1->2->5, 1->3
```c++
class Solution {
public:
vector<string> binaryTreePaths(TreeNode* root) {
    vector<string> res;
    help(res, root, "");
    return res;
}
void help(vector<string>& res, TreeNode* root, string pre) {
    if (!root)
        return;
    if (!root->left && !root->right) {
        res.push_back(pre + to_string(root->val));
        return;
    }
    help(res, root->left, pre + to_string(root->val) + "->");
    help(res, root->right, pre + to_string(root->val) + "->");
}
};
```