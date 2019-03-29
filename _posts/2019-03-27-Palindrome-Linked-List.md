---
layout: post
title:  "234. Palindrome Linked List"
date: 2019-03-27 19:45:23 -0400
categories: articles
---
Given a singly linked list, determine if it is a palindrome.

Example 1:
```
Input: 1->2
Output: false
```
Example 2:
```
Input: 1->2->2->1
Output: true
```
Follow up:
Could you do it in O(n) time and O(1) space?
```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        ListNode* mid = head, *fast = head;
        if ( !head ) return true;
        // Get middle
        while (fast && fast->next){
            mid = mid->next;
            fast = fast->next->next;
        }
        // reverse second half
        ListNode* shead = fast ? mid->next : mid ;
        ListNode* curr = shead;
        while ( curr && curr->next ) {
            ListNode* next = curr->next;
            curr->next = next->next;
            next->next = shead;
            shead = next;
        }
        
        curr = head;
        while( shead ){
            if (curr->val != shead->val) return false;
            curr = curr->next;
            shead = shead->next;
        }
        return true;
    }
};
```

```c++
// Reverse Linked List
        ListNode* curr = shead;
        while ( curr && curr->next ) {
            ListNode* next = curr->next;
            curr->next = next->next;
            next->next = shead;
            shead = next;
        }
```