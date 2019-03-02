---
layout: post
title:  "147. Insertion Sort List"
date: 2019-02-28 08:13:23 -0400
categories: articles
---
Algorithm of Insertion Sort:

Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
It repeats until no input elements remain.

Example 1:
```
Input: 4->2->1->3
Output: 1->2->3->4
```
Example 2:
```
Input: -1->5->3->4->0
Output: -1->0->3->4->5
```

# My Wrong Answer
```c++
class Solution {
public:
    ListNode* insertionSortList(ListNode* head) {
        if (!head) return head;
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* curr = head->next;
        ListNode* idx = dummy;
        while( curr ){
            idx = dummy;
            ListNode* next;
            while (idx->next != curr){
            if (curr->val < idx->next->val){
                next = curr->next;
                curr->next = idx->next;
                idx->next = curr;
                break;
            }
            else{ 
                idx = idx->next; 
            }
                curr = next;
            }
        }
        return dummy->next;
    }
};
```
# Good Answer
```c++
class Solution { 
public:
    ListNode* insertionSortList(ListNode* head) {
        ListNode* new_head = new ListNode(0);
        new_head -> next = head;
        ListNode* pre = new_head;
        ListNode* cur = head;
        while (cur) {
            if (cur -> next && cur -> next -> val < cur -> val) {
                while (pre -> next && pre -> next -> val < cur -> next -> val)
                    pre = pre -> next;
                /* Insert cur -> next after pre.*/
                ListNode* temp = pre -> next;
                pre -> next = cur -> next;
                cur -> next = cur -> next -> next;
                pre -> next -> next = temp;
                /* Move pre back to new_head. */
                pre = new_head;
            }
            else cur = cur -> next;
        }
        ListNode* res = new_head -> next;
        delete new_head;
        return res;
    }
};
```