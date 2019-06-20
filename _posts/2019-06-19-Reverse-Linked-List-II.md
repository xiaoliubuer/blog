---
layout: post
title:  "92. Reverse Linked List II"
date: 2019-06-19 08:31:00 -0400
categories: articles
---
Reverse a linked list from position m to n. Do it in one-pass.

Note: 1 ≤ m ≤ n ≤ length of list.

Example:
```
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
```
```c++
// Eaiser understand
class Solution {
public:
ListNode* reverseBetween(ListNode *head, int m, int n) {
    ListNode* dummy = new ListNode( 0), *prev = dummy;
    dummy->next = head;
    for ( int i = 1; i < m; i++ )
        prev = prev->next; // Ensure prev's next points to the current node
    ListNode *curr = prev->next;
    for ( int i = m; i < n; i++ ) {
        ListNode* next = curr->next;
        curr->next = next->next;
        next->next = prev->next;
        prev->next = next;
    }
    return dummy->next;
}
};
```
```c++
class Solution {
public:
ListNode* reverseBetween(ListNode *head, int m, int n) {
    ListNode* dummy = new ListNode(0), *prev = dummy;
    dummy->next = head;
    for ( int i = 1; i < m; i++ )
        prev = prev->next; // Ensure prev's next points to the current node
    ListNode *curr = prev->next;
    for ( int i = m; i < n; i++ ) {
        swap(prev->next, curr->next->next);
        swap(prev->next, curr->next);
    }
    return dummy->next;
}
};
```