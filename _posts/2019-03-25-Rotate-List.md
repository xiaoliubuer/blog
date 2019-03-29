---
layout: post
title:  "61. Rotate List"
date: 2019-03-25 12:46:23 -0400
categories: articles
---
Given a linked list, rotate the list to the right by k places, where k is non-negative.

Example 1:
```
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
```
Example 2:
```
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
```

# My answer
```c++
// Accepted !!! 03/25/2019
class Solution {
public:
    ListNode* getTail(ListNode* head){
        if ( !head || !head->next ) return head;
        while (head->next->next) head = head->next;
        return head;
    }
    ListNode* rotateRight(ListNode* head, int k) {
        if ( !head || !head->next ) return head;
        int len = 0;
        ListNode* curr = head;
        while ( curr ){
            curr = curr->next;
            len++;
        }
        k = k % len;
        
        while ( k > 0) {
            ListNode* tail = getTail(head);
            ListNode* next = tail->next;
            next->next = head;
            head = next;
            tail->next = nullptr;
            k--;
        }
        return head;
    }
};
```

```c++
// Better Answer
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
    	if (head == NULL) return head;

    	int len = 0;
    	for (ListNode *node = head; node != NULL; node = node->next){
    		len++;
    	}
    	k = k % len;
    	if (k == 0) return head;

    	ListNode *fast = head;
    	for (int i = 0; i < k; ++i) fast = fast->next;

    	ListNode *slow = head;
    	while(fast->next != NULL){
    		slow = slow->next;
    		fast = fast->next;
    	}
    	fast->next = head;
    	head = slow->next;
    	slow->next = NULL;
    	return head;
    }
};
```