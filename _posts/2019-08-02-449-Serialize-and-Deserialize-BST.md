---
layout: post
title:  "449. Serialize and Deserialize BST"
date: 2019-08-02 18:54:00 -0400
categories: articles
---
Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary search tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary search tree can be serialized to a string and this string can be deserialized to the original tree structure.

The encoded string should be as compact as possible.

Note: Do not use class member/global/static variables to store states. Your serialize and deserialize algorithms should be stateless.

```c++
class Codec {
private:
    void addNodeToString(stringstream& ss, TreeNode* ptrNode) {
        if (ptrNode == nullptr) return;
        ss << ptrNode->val << ' ';
        addNodeToString(ss, ptrNode->left);
        addNodeToString(ss, ptrNode->right);
    }
    
    void recInsert(const int& i, TreeNode*& ptrNode) {  
        if (ptrNode == nullptr) {
            ptrNode = new TreeNode(i);
            return;
        }
        if (i < ptrNode->val) recInsert(i, ptrNode->left);
        else recInsert(i, ptrNode->right);
    }
    
public:

    string serialize(TreeNode* root) {
        stringstream ss;
        addNodeToString(ss, root);
        return ss.str();
    }

    TreeNode* deserialize(string data) {        
        TreeNode* root = nullptr;
        stringstream ss { data };
        for (int i; ss >> i; ){   
            recInsert(i, root);
        }
        return root;
    }
};
```