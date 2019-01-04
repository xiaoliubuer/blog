---
layout: post
title:  "19. Remove Nth Node From End of List"
date: 2018-12-31 15:17:23 -0400
categories: articles
---

Given a linked list, remove the n-th node from the end of list and return its head.

Example:
```
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.
```
Note:

Given n will always be valid.

Follow up:

Could you do this in one pass?

# 题意
给一个链表，删除从尾巴上数的第n个元素。返回链表的头指针。
因为链表只能从头往后走。所以删除从尾向头的第N个。

# function signature
```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        
    }
};
```
# 思路
1. Get the length of the list
先遍历一次，算出链表的长度。
```c++
ListNode* temp = head;
int len = 0;
while (temp) {
	len++;
}
```
2. length - n
```c++
int res = len - n - 1;
```
3. Move pointer to the place then remove the specific node
```c++
temp = head;
while ( res > 0){
	temp = temp -> next;
}
next = temp->next;
temp->next = next->next;
delete next;
```
4. return head
```c++
return head;
```
# 参考答案
```c++
// 方法一: 用数字标记遍历了多少
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
		ListNode* temp = head;
		int len = 0;
		while (temp) {
			len++;
            temp = temp ->next;
		}
		int res = len - n;
        ListNode* dummy = new ListNode(0);
		dummy->next =  head;
        temp = dummy;
		while ( res > 0){
			temp = temp -> next;
            res--;
		}
		ListNode* next = temp->next;
		temp->next = next->next;
		delete next;
		return dummy->next;        
    }
};
```
```c++
// 方法二:
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
    	if (!head) return head;
    	ListNode* fast = head;
    	for (int i = 0; i < n && fast; ++i){
    		fast = fast->next;
    	}

    	ListNode* dummy = new ListNode(0);
    	dummy->next = head;

    	ListNode* temp = dummy;
    	while(fast){
    		fast = fast->next;
    		temp = temp->next;
    	}
    	temp->next = temp->next->next;

        return dummy->next;
    }
};
```