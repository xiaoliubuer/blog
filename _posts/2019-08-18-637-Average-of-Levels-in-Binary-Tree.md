---
layout: post
title:  "637. Average of Levels in Binary Tree"
date: 2019-08-18 15:02:00 -0400
categories: articles
---

Given a non-empty binary tree, return the average value of the nodes on each level in the form of an array.
Example 1:
```
Input:
    3
   / \
  9  20
    /  \
   15   7
Output: [3, 14.5, 11]
```
Explanation:
```
The average value of nodes on level 0 is 3,  on level 1 is 14.5, and on level 2 is 11. Hence return [3, 14.5, 11].
```
Note:
```
The range of node's value is in the range of 32-bit signed integer.
```
```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        map<int,double> lsum;
        map<int,int> cnt;
        store(root, 1, lsum, cnt);
        vector<double> res;
        for(auto kv: lsum) {
            res.push_back((kv.second)/cnt[kv.first]);
        }
        return res;
    }
    void store(TreeNode *root, int l, map<int,double> &lsum, map<int,int> &cnt) {
        if(root == NULL) return ;
        
        lsum[l] += root->val;
        cnt[l] ++;
        
        store(root->left, l+1, lsum, cnt);
        store(root->right, l+1, lsum, cnt);
    }
};
```
```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        queue<TreeNode *> q;
        vector<double> v;
        if (!root) return v;
        double cur_sum=0;
        int k=0;
        q.push(root);
        q.push(NULL);
        while(!q.empty()){
            TreeNode *front=q.front();
            q.pop();
            if(!front){
                v.push_back(cur_sum/k);
                cur_sum=0;
                k=0;
                if(!q.empty())
                    q.push(NULL);
            }
            else{
                cur_sum+=front->val;
                k++;
                if(front->left) q.push(front->left);
                if(front->right) q.push(front->right);
            }
            
        }
        return v;
    }
};
```
```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<pair<double, int>> v;
        vector<double> ans;
        order(root, v, 0);
        for (auto it = v.begin(); it != v.end(); it++){
            ans.push_back(it->first / it->second);
        }
        return ans;
    }
    void order(TreeNode* node, vector<pair<double, int>>& v, int index) {
        if(node == nullptr) return;
        if(v.size() > index) {
            v[index].first += node->val;
            v[index].second++;
        }
        else {
            v.push_back(std::make_pair(node->val, 1));
        }
        order(node->left, v, index + 1);
        order(node->right,  v, index + 1);
    }
};
```
```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<double> ans;
        queue<TreeNode* > myqueue;
        if (!root)
            return ans;
        myqueue.push(root);
        while(!myqueue.empty())
        {
            int n = myqueue.size();
            double sum = 0.0;
            for (int i=0;i<n;i++){
            
            TreeNode* ptr = myqueue.front();
            myqueue.pop();
            sum+=ptr->val;
            if (ptr->left)
                myqueue.push(ptr->left);
            if (ptr->right)
                myqueue.push(ptr->right);
            }
            sum/=n;
            ans.push_back(sum);
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    vector<double> averageOfLevels(TreeNode* root) {
        vector<pair<double, int>> v;
        vector<double> ans;
        order(root, v, 0);
        for (auto it = v.begin(); it != v.end(); it++){
            ans.push_back(it->first / it->second);
        }
        return ans;
    }
    void order(TreeNode* node, vector<pair<double, int>>& v, int index) {
        if(node == nullptr) return;
        if(v.size() > index) {
            v[index].first += node->val;
            v[index].second++;
        }
        else {
            v.push_back(std::make_pair(node->val, 1));
        }
        order(node->left, v, index + 1);
        order(node->right,  v, index + 1);
    }
};
```