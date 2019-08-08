---
layout: post
title:  "508. Most Frequent Subtree Sum"
date: 2019-08-07 20:00:00 -0400
categories: articles
---
Given the root of a tree, you are asked to find the most frequent subtree sum. The subtree sum of a node is defined as the sum of all the node values formed by the subtree rooted at that node (including the node itself). So what is the most frequent subtree sum value? If there is a tie, return all the values with the highest frequency in any order.

Examples 1
```
Input:

  5
 /  \
2   -3
return [2, -3, 4], since all the values happen only once, return all of them in any order.
```
Examples 2
```
Input:

  5
 /  \
2   -5
return [2], since 2 happens twice, however -5 only occur once.
```
Note: You may assume the sum of values in any subtree is in the range of 32-bit signed integer.

```c++
class Solution {
public:
    int traverse (TreeNode* root, unordered_map<int, int> &mp) {
        if (root == NULL) return 0;
        int left = traverse(root->left, mp);
        int right = traverse(root->right, mp);
        int cur = left + right + root->val;
        mp[cur]++;
        return cur;
    }
    
    vector<int> findFrequentTreeSum(TreeNode* root) {
        unordered_map<int, int> mp;
        
        traverse(root, mp);
        
        unordered_map<int,int>::iterator it = mp.begin();
        
        int max = INT_MIN;
        
        while (it != mp.end()) {
            if (it->second > max) max = it->second;
            it++;
        }
        
        vector<int> ans;
        it = mp.begin();
        while (it != mp.end()) {
            if (it->second == max) ans.push_back(it->first);
            it++;
        }
        
        return ans;
    }
};
```
```c++
class Solution {
public:
    vector<int> findFrequentTreeSum(TreeNode* root) {
        map<int, int> cnt;
        vector<int> T;
        int mx = 0;
        sum(root, cnt, mx);
        for(auto itr = cnt.begin(); itr != cnt.end(); itr++){
            if((*itr).second == mx){
                T.push_back((*itr).first);
            }
        }
        
        return T;
        
    }
    
    int sum(TreeNode* root, map<int,int>& cnt, int& mx){
        if(root == NULL) return 0;
        int ret = root->val + sum(root->left, cnt, mx) + sum(root->right, cnt, mx);
        cnt[ret]++;
        mx = mx > cnt[ret] ? mx : cnt[ret];
        // mx = max(mx, cnt[ret]);
        return ret;
    }
    
};
```
```c++
class Solution {
public:
    vector<int> findFrequentTreeSum(TreeNode* root) {
        map<int, int> cnt;
        vector<int> T;
        int mx = 0;
        sum(root, cnt, mx);
        for(auto itr = cnt.begin(); itr != cnt.end(); itr++){
            if((*itr).second == mx){
                T.push_back((*itr).first);
            }
        }
        
        return T;
        
    }
    
    int sum(TreeNode* root, map<int,int>& cnt, int& mx){
        if(root == NULL) return 0;
        int ret = root->val + sum(root->left, cnt, mx) + sum(root->right, cnt, mx);
        cnt[ret]++;
        mx = mx > cnt[ret] ? mx : cnt[ret];
        // mx = max(mx, cnt[ret]);
        return ret;
    }
    
};
```
```c++
class Solution {
public:
    map<int,int>m;
    int storesum(TreeNode*root,map<int,int>&m)
    {
        if(root==NULL) return 0;
        int ls=storesum(root->left,m); // store sum of left subtree
        int rs=storesum(root->right,m); //store sum of right subtree
        m[ls+rs+root->val]++; //update the freq of the  sum of subtree
       return root->val+ls+rs;
    }
    vector<int> findFrequentTreeSum(TreeNode* root) {
        vector<int>ans;
        if(root==NULL) return ans;
        storesum(root,m);
        int maxfreq=INT_MIN;
        for(auto itr=m.begin();itr!=m.end();itr++)
        {
            if(m[itr->first]>maxfreq)
            {
                ans.clear(); // delete previous elements of lower freq
                ans.push_back(itr->first);  // add the current element of higher freq
                maxfreq=m[itr->first]; // update max freq
            }
            else if(m[itr->first]==maxfreq)
            {
                ans.push_back(itr->first); // if tie in freq, just add the element to the vector
            }
        }
        return ans;
    }
};
```