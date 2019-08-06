---
layout: post
title:  "426. Convert Binary Search Tree to Sorted Doubly Linked List"
date: 2019-08-02 19:41:00 -0400
categories: articles
---

Convert a BST to a sorted circular doubly-linked list in-place. Think of the left and right pointers as synonymous to the previous and next pointers in a doubly-linked list.

Let's take the following BST as an example, it may help you understand the problem better:

We want to transform this BST into a circular doubly linked list. Each node in a doubly linked list has a predecessor and successor. For a circular doubly linked list, the predecessor of the first element is the last element, and the successor of the last element is the first element.

The figure below shows the circular doubly linked list for the BST above. The "head" symbol means the node it points to is the smallest element of the linked list.

Specifically, we want to do the transformation in place. After the transformation, the left pointer of the tree node should point to its predecessor, and the right pointer should point to its successor. We should return the pointer to the first element of the linked list.

The figure below shows the transformed BST. The solid line indicates the successor relationship, while the dashed line means the predecessor relationship.

```c++
class Solution {
public:

    Node* treeToDoublyList(Node* root) {
        if(!root)
            return root;
        Node *lefthead = treeToDoublyList(root->left);
        Node *righthead = treeToDoublyList(root->right);
        root->left = root;
        root->right = root;
        return connect(connect(lefthead, root), righthead);
    }
    
    Node *connect(Node *n1, Node *n2) {
        if(!n1)
            return n2;
        if(!n2)
            return n1;
        Node *tail1 = n1->left, *tail2 = n2->left;
        tail1->right = n2;
        n2->left = tail1;
        n1->left = tail2;
        tail2->right = n1;
        return n1;
    }
};
```
```c++
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if(!root) return NULL;
        Node* dummy = new Node(0);
        auto cur = dummy;
        stack<Node*> st;
        auto p = root;
        while(p || !st.empty()){
            while(p){
                st.push(p);
                p = p->left;
            }
            p = st.top();
            st.pop();
            cur->right = p;
            p->left = cur;
            cur = p;
            p = p->right;
        }
        auto res = dummy->right;
        res->left = cur;
        cur->right = res;
        return res;
    }
};
```
```c++
class Solution {
public:
    Node* treeToDoublyList(Node* root) {
        if(!root) return NULL;
        Node *dummy = new Node(-1, NULL, NULL);
        Node *pre = dummy;
        helper(root, pre);
        pre->right = dummy->right;
        dummy->right->left = pre;
        return dummy->right;
        
    }
    void helper(Node *root, Node* &pre){
        if(root->left) helper(root->left, pre);
        pre->right = root;
        root->left = pre;
        pre = pre->right;
        if(root->right) helper(root->right, pre);
    }
};
```
```c++
class Solution {
    Node *first = NULL, *last = NULL;
    private:
    void treeDFS(Node * root){
        if(root){
            treeDFS(root->left);
            if(last != NULL){
                root->left = last;
                last->right = root;
            }
            else{
                first = root;
            }
            last = root;
            treeDFS(root->right);
        } 
    }
    
public:
    Node* treeToDoublyList(Node* root) {
        if(!root) return root;
        treeDFS(root);
        first->left = last;
        last->right = first;
        return first;
    }
};
```
```c++
class Solution {
public:
    Node* head=NULL;
    Node* tail=NULL;
    Node* treeToDoublyList(Node* root) {
        if(!root) return NULL;
        dfs(root);
        head->left=tail;
        tail->right=head;
        return head;
    }
    void dfs(Node* root){
        if(!root) return;
        dfs(root->left);     
        if(!head) // find the leftmost as head
            head=root;
        else { // when head is found, tail is also set
            root->left=tail;
            tail->right=root; 
        }
        tail=root;
        dfs(root->right);
    }
};
```