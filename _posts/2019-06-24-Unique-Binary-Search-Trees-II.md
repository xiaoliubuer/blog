---
layout: post
title:  "95. Unique Binary Search Trees II"
date: 2019-06-24 19:37:00 -0400
categories: articles
---
Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1 ... n.

Example:
```
Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]
Explanation:
The above output corresponds to the 5 unique BST's shown below:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```
```c++
class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return {};
        return helper(1, n);
    }
    
    vector<TreeNode*> helper(int start, int end) {
        if (start > end) return {nullptr};
        vector<TreeNode*> res;
        for (int i = start; i <= end; ++i) {
            auto left = helper(start, i - 1), right = helper(i + 1, end);
            for (auto a : left) {
                for (auto b : right) {
                    TreeNode *node = new TreeNode(i);
                    node->left = a;
                    node->right = b;
                    res.push_back(node);
                }
            }
        }
        return res;
    }
};
```
```c++
class Solution {
private:
    unordered_map<string, vector<TreeNode*>> map;
    
public:
    vector<TreeNode*> generateTrees(int n) {
        if (n == 0) return {};
        return helper(1, n);
    }
    
    vector<TreeNode*> helper(int start, int end) {
        vector<TreeNode*> result;
        if (start > end) {
            result.push_back(nullptr);
            return result;
        }
        if (start == end) {
            TreeNode * node = new TreeNode(start);
            result.push_back(node);
            return result;
        }
        string val = to_string(start) + "_" + to_string(end);
        if (map.count(val) > 0) return map[val];
        
        for (int i = start; i <= end; i++) {
            vector<TreeNode*> left = helper(start, i - 1);
            vector<TreeNode*> right = helper(i + 1, end);
            for (int j = 0; j < left.size(); j++) {
                for (int k = 0; k < right.size(); k++) {
                    TreeNode * node = new TreeNode(i);
                    node->left = left[j];
                    node->right = right[k];
                    result.push_back(node);
                }
            }
        }
        
        map[val] = result;
        return result;
    }
};
```