---
layout: post
title:  "142. Linked List Cycle II"
date: 2019-03-30 13:13:23 -0400
categories: articles
---

Given a linked list, return the node where the cycle begins. If there is no cycle, return null.

To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.

Note: Do not modify the linked list.

 

Example 1:

Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.

```c++
// Accepted!!
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow = head;
        ListNode* fast = head;
        while( fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
            if ( slow == fast ) break;
        }
        fast = head;
        while ( slow && slow->next ){
            if (slow == fast) return slow;
            slow = slow->next;
            fast = fast->next;
        }
        return nullptr;
    }
};
```