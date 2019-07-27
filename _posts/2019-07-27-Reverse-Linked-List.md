---
layout: post
title:  "206. Reverse Linked List"
date: 2019-07-27 18:32:00 -0400
categories: articles
---
Reverse a singly linked list.

Example:
```
Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL
```
Follow up:
```
A linked list can be reversed either iteratively or recursively. Could you implement both?
```
```c++
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if ( !head ) return head; //Return if head is NULL
        ListNode* curr = head;
        ListNode* next = curr->next;
        
        while ( curr && next ){
            curr->next = next->next;
            next->next = head;
            head = next;
            next = curr->next;
        }
        return head;
    }
};
```