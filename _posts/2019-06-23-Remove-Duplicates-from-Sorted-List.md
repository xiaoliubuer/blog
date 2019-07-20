---
layout: post
title:  "83. Remove Duplicates from Sorted List"
date: 2019-06-23 19:17:00 -0400
categories: articles
---
Given a sorted linked list, delete all duplicates such that each element appear only once.

Example 1:
```
Input: 1->1->2
Output: 1->2
```
Example 2:
```
Input: 1->1->2->3->3
Output: 1->2->3
```

```c++
// Genius answer!!
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode *p = head;
        while( p && p->next ) {
            if ( p->val == p->next->val )
                p->next = p->next->next;
            else
                p = p->next;
        }
        return head;
    }
};
```
```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode *tmp = head;
        ListNode *next = head;
        if( NULL !=tmp )
            next = tmp->next;
        else
            return NULL;
            
        while(next){
            if(tmp->val == next->val){
                tmp->next = next->next;
                next->next = NULL;
                delete next;
                next = tmp->next;
            }
            else{
                tmp = next;
                next = next->next;
            }
        }
        return head;
    }
};
```