---
layout: post
title:  "421. Maximum XOR of Two Numbers in an Array"
date: 2019-08-01 21:12:00 -0400
categories: articles
---
Given a non-empty array of numbers, a0, a1, a2, … , an-1, where 0 ≤ ai < 231.

Find the maximum result of ai XOR aj, where 0 ≤ i, j < n.

Could you do this in O(n) runtime?

Example:
```
Input: [3, 10, 5, 25, 2, 8]

Output: 28

Explanation: The maximum result is 5 ^ 25 = 28.
```
```c++
struct TrieNode{ //实际是TreeNode struct  O(n^m)
    int val;
    TrieNode *left;
    TrieNode *right;
    TrieNode(int x) : val(x), left(NULL), right(NULL) {}
};

class Solution {
public:
    //DISUCSSION: 使用前缀树(二叉树表示) 打败 92%
    int findMaximumXOR(vector<int>& nums) {
        TrieNode* root = new TrieNode(0);


        //建树
        TrieNode* curNode = root;
        for(int i = 0; i < nums.size(); i++){
            for(int j = 31; j >= 0; j--) {
                int tmp = nums[i] & (1 << j);
                if(tmp == 0){
                    if(!curNode->right){
                        curNode->right = new TrieNode(0);//建个新的
                    }
                    curNode = curNode->right; //继续往下走
                }else{
                    if(!curNode->left){
                        curNode->left = new TrieNode(1);
                    }
                    curNode = curNode->left;
                }
            }
            curNode = root;//回到头
        }

        //匹配最大异或值
        int max = 0;
        for(int i = 0; i < nums.size(); i++){
            int res = 0;
            for(int j = 31; j >= 0; j--){  //一层层往下来
                int tmp = nums[i] & (1 << j);
                //cout << (1 << j) << "\t" << tmp << endl;
                if(curNode->left && curNode->right){
                    if(tmp == 0){
                        curNode = curNode->left;
                    }else {
                        curNode = curNode->right;
                    }    
                }else {
                    curNode = curNode->left == NULL ? curNode->right:curNode->left; //没有选择 只有一条
                }
                res += tmp ^ (curNode->val << j);
                //cout << curNode->val;
            }
            curNode = root;
            //cout << endl;
            max = max > res?max:res;
        }

        return max;
    }
};
```
```c++
class Solution {
public:
    int findMaximumXOR(vector<int>& nums) {
        node* root = new node;
        for (auto num : nums) {
            insert(root, num);
        }
        int res = 0;
        for (auto num: nums) {
            res = max(res, compare(root, num));
        }        
        delete root;
        return res;
    }
private: 
    class node {
    public:
        node(): zero(nullptr), one(nullptr) {}
        ~node() { delete zero; delete one; }
        node* zero;
        node* one; 
    };
    void insert(node* root, int num) {
        node* p = root;
        for (int i = 31; i > 0; --i) {
            bool is_one = num & (1 << (i-1));
            if (is_one) {
                if (!p->one) {
                    p->one = new node;
                }
                p = p->one;                                    
            }
            else { 
                if (!p->zero) {
                    p->zero = new node;
                }
                p = p->zero;
            }            
        }
    }
    int compare(node* root, int num) {
        int res = 0;
        node* p = root; 
        for (int i = 31; i > 0; --i) {
            bool is_one = num & (1 << (i-1));
            if (is_one) {
                if (p->zero) {
                  res += (1 << (i-1));   
                  p = p->zero; 
                } else {
                    p = p->one;                    
                }                
            } else { 
                if (p->one) {
                    res += (1 << (i-1));
                    p = p->one;
                } else {
                    p = p->zero;
                }            
            }
            
        }
        return res;        
    }    
};
```