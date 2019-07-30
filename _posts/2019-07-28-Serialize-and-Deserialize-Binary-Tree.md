---
layout: post
title:  "297. Serialize and Deserialize Binary Tree"
date: 2019-07-28 20:18:00 -0400
categories: articles
---
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

Example: 
```
You may serialize the following tree:

    1
   / \
  2   3
     / \
    4   5

as "[1,2,3,null,null,4,5]"
```
Clarification: The above format is the same as how LeetCode serializes a binary tree. You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

```c++
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        if (root == nullptr) return "#";
        return to_string(root->val)+","+serialize(root->left)+","+serialize(root->right);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        return mydeserialize(data);
    }
    TreeNode* mydeserialize(string& data) {
        if (data[0]=='#') {
            if(data.size() > 1) data = data.substr(2);
            return nullptr;
        } else {
            TreeNode* node = new TreeNode(helper(data));
            node->left = mydeserialize(data);
            node->right = mydeserialize(data);
            return node;
        }
    }
private:
    int helper(string& data) {
        int pos = data.find(',');
        int val = stoi(data.substr(0,pos));
        data = data.substr(pos+1);
        return val;
    }
};`
```
```c++
class Codec {
private:
  void shelper(TreeNode* node, string& data) {
    if (node == nullptr) {
      data += "null,";
      return;
    }
    data += to_string(node->val) + ",";
    shelper(node->left, data);
    shelper(node->right, data);
  }
  void dhelper(TreeNode*& node, int& index, vector<string>& datav) {
    if (datav[index] == "null") {
      ++index;
      return;
    }
    node = new TreeNode(atoi(datav[index++].c_str()));
    TreeNode* left = nullptr, *right = nullptr;
    dhelper(left, index, datav);
    dhelper(right, index, datav);
    node->left = left;
    node->right = right;
  }
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
      string data = "";
      shelper(root, data);
      return data;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
      if (data == "") return nullptr;
      int prep = 0, p = data.find(',', prep);
      vector<string> datav{};
      while (p != string::npos) {
        datav.push_back(data.substr(prep, p - prep));
        prep = p + 1;
        p = data.find(',', prep);
      }
      TreeNode* head = nullptr;
      int index = 0;
      dhelper(head, index, datav);
      
      return head;
    }
};
```