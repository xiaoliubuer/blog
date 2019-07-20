---
layout: post
title:  "82. Remove Duplicates from Sorted List II"
date: 2019-06-23 19:15:00 -0400
categories: articles
---

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.

Example 1:
```
Input: 1->2->3->3->4->4->5
Output: 1->2->5
```
Example 2:
```
Input: 1->1->1->2->3
Output: 2->3
```
```c++
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) return head;
        if (head->val != head->next->val) {
            head->next = deleteDuplicates(head->next);
            return head;
        }
        ListNode* next = head->next->next;
        while (next && next->val == head->val)
            next = next->next;
        return deleteDuplicates(next);
    }
};
```