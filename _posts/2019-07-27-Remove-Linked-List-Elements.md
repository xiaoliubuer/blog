---
layout: post
title:  "203. Remove Linked List Elements"
date: 2019-07-27 18:26:00 -0400
categories: articles
---
Remove all elements from a linked list of integers that have value val.

Example:
```
Input:  1->2->6->3->4->5->6, val = 6
Output: 1->2->3->4->5
```
```c++
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
		if (!head) return head;

		ListNode* dummy = new ListNode(0);
		dummy->next = head;
		ListNode* temp = dummy;
		while( temp->next != NULL ){
			if ( temp->next->val == val ){
				temp->next = temp->next->next;
			}
			else
                temp = temp->next;
		}     
		return dummy->next;
    }
};
```