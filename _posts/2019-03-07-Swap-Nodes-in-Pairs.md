---
layout: post
title:  "24. Swap Nodes in Pairs"
date: 2019-03-07 20:08:23 -0400
categories: articles
---
Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed.

Example:
```
Given 1->2->3->4, you should return the list as 2->1->4->3.
```

```c++
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        if (!head) return head;
        
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* pre = dummy;
        
        ListNode* p1 = head;
        ListNode* p2 = head->next;
        
        while (p1 && p2) {
            pre->next = p2;
            p1->next = p2->next;
            p2->next = p1;
            pre = p1;
            
            if (p1) p1 = p1->next;
            if (p1) p2 = p1->next;
            else p2 = p1;
        }
        return dummy->next;
    }
};
```