---
layout: post
title:  "369. Plus One Linked List"
date: 2019-07-31 20:23:00 -0400
categories: articles
---
Given a non-negative integer represented as non-empty a singly linked list of digits, plus one to the integer.

You may assume the integer do not contain any leading zero, except the number 0 itself.

The digits are stored such that the most significant digit is at the head of the list.

Example :
```
Input: [1,2,3]
Output: [1,2,4]
```
```c++
class Solution {
public:
    ListNode* plusOne(ListNode* head) {
        int val = Helper(head);
        if (val == 1){
            ListNode *nhead = new ListNode(1);
            nhead->next = head;
            return nhead;
        }
        return head;
    }
    
    int Helper(ListNode* node){
        if (!node) return 1;
        
        int val = Helper(node->next);
        if (val){
        	if (node->val == 9){
                node->val = 0;
                return 1;
            } else{
                node->val++;
                return 0;
            }
        }
        else
            return 0;
    }
};
```
```c++
class Solution {
private:
    int compute(ListNode* head){
        ListNode* curr=head;
        if (!curr->next)
            curr->val++;
        else
            curr->val+=compute(head->next);
            
        if (curr->val>=10){
            curr->val-=10;
            return 1;
        }
        return 0;
    }
public:
    ListNode* plusOne(ListNode* head) {
        if (!head) return head;
        if (compute(head)==1) {
            ListNode* temp = head;
            ListNode* preHead = new ListNode(1);
            preHead->next = temp;
            head = preHead;
        }
        return head;
    }
};
```