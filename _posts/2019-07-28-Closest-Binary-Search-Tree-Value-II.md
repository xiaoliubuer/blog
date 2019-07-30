---
layout: post
title:  "272. Closest Binary Search Tree Value II"
date: 2019-07-28 19:33:00 -0400
categories: articles
---

Given a non-empty binary search tree and a target value, find k values in the BST that are closest to the target.

Note:
Given target value is a floating point.
You may assume k is always valid, that is: k ≤ total nodes.
You are guaranteed to have only one unique set of k values in the BST that are closest to the target.
Example:
```
Input: root = [4,2,5,1,3], target = 3.714286, and k = 2

    4
   / \
  2   5
 / \
1   3

Output: [4,3]
```
Follow up:
Assume that the BST is balanced, could you solve it in less than O(n) runtime (where n = total nodes)?

```c++
class Solution {
public:
    vector<int> closestKValues(TreeNode* root, double target, int k) {
        deque<int> dq;
        vector<int> res;
        dfs(root, target, k, dq);
        for(int i : dq) res.push_back(i);
        return res;
    }
    void dfs(TreeNode* root, double target, int k, deque<int>& dq) {
        if(!root) return;
        dfs(root->left, target, k, dq);
        
        if(dq.size() == k) {
            if(fabs(root->val - target) < fabs(dq.front() - target)) {
                dq.pop_front();
                dq.push_back(root->val);
            } else return; // pruning
        } else dq.push_back(root->val);
        
        dfs(root->right, target, k, dq);
    }
};
```
```c++
void dfs(TreeNode* root, priority_queue<pair<double, int>>& pq, double target, int k) {
    if(!root) return;
    
    pq.push(make_pair(fabs(target - double(root->val)), root->val));
    
    if(pq.size() > k) 
        pq.pop();
        
    dfs(root->left, pq, target, k);
    dfs(root->right, pq, target, k);
}

vector<int> closestKValues(TreeNode* root, double target, int k) {
    priority_queue<pair<double, int>> pq;
    vector<int> result;
    
    dfs(root, pq, target, k);
    while(!pq.empty()) {
        result.push_back(pq.top().second);
        pq.pop();
    }
    
    return result;
}
```