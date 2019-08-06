---
layout: post
title:  "430. Flatten a Multilevel Doubly Linked List"
date: 2019-08-02 19:57:00 -0400
categories: articles
---
You are given a doubly linked list which in addition to the next and previous pointers, it could have a child pointer, which may or may not point to a separate doubly linked list. These child lists may have one or more children of their own, and so on, to produce a multilevel data structure, as shown in the example below.

Flatten the list so that all the nodes appear in a single-level, doubly linked list. You are given the head of the first level of the list.
Example:
```
Input:
 1---2---3---4---5---6--NULL
         |
         7---8---9---10--NULL
             |
             11--12--NULL

Output:
1-2-3-7-8-11-12-9-10-4-5-6-NULL
```

Explanation for the above example:

Given the following multilevel doubly linked list:
We should return the following flattened doubly linked list:

```c++
class Solution {
public:
    Node* flatten(Node* head) {
        helper(head);
        return head;
    }
    
    Node * helper(Node * head){
        Node * prev = NULL;
        while(head != NULL){
            
            if(head->child != NULL){
                Node * next = head->next;
                
                Node * end = helper(head->child);
                head->next = head->child;
                head->next->prev = head;
                head->child = NULL;
                
                end->next = next;
                if(next != NULL){
                    next->prev = end;
                }
                
                prev = end;
                head = next;   
            }else{
                prev = head;
                head = head->next;
            }
        }
        
        return prev;
    }
};
```
```c++
class Solution {
public:
    Node* flatten(Node* head) {
        Node* cur = head;
        while (cur) {
            if (cur->child) {
                Node *next = cur->next;
                cur->child = flatten(cur->child);
                Node* last = cur->child;
                while(last->next) last = last->next;
                cur->next = cur->child;
                cur->next->prev = cur;
                cur->child = NULL;
                last->next = next;
                if (next) next->prev = last;
                cur = last;
            }
            cur = cur->next;
        }
        return head;
    }
};
```
```c++
class Solution {
public:
    Node* flatten(Node* head) {
        if (head == NULL || (head->next == NULL && head->child == NULL)) return head;
        Node* curr = head;
        stack<Node*> nodes;
        while (curr) {
            if (curr->child != NULL) {
                nodes.push(curr->next);
                curr->next = curr->child;
                curr->next->prev = curr;
                curr->child = NULL;
            } else if (curr->next == NULL && !nodes.empty()) {
                curr->next = nodes.top();
                nodes.pop();
                if (curr->next) curr->next->prev = curr;
            }
            curr = curr->next;
        }
        
        return head;
    }
};
```