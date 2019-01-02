---
layout: post
title:  "02. Add Two Numbers"
date: 2019-01-01 07:07:23 -0400
categories: articles
---
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

Example:
```
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.
```
# 题意
两个非空链表，代表两个非负数，数字被反着存着，并且每个节点存一个单独的数字，把两个数字加起来，和返回一个链表。

# Function signature
```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        
    }
};
```
# 思路
两个指针分别指向两个list, 只要不空，就往后走。
定义一个carry，来保存进位。每次两个数字相加需要加carry.
如果有一个指针为空，循环结束，那么就找到那个不为空的。继续循环那个不为空的，知道结束。
判断carry此时是否为1，如果是，就在新建一个node，赋值1，补到链表最后端。
最后返回头指针。

# 参考答案
```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
    	if (!l1) return l2;
    	if (!l2) return l1;

    	ListNode* dummy = new ListNode(0);
    	ListNode* curr = dummy;
    	int carry = 0;
    	while(l1 || l2){
    		int n1 = l1 ? l1 -> val : 0;
    		int n2 = l2 ? l2 -> val : 0;
    		int sum = n1 + n2 + carry;
    		if ( sum > 9 ) {
    			sum = sum % 10;
    			carry = 1;
    		}
    		else
    			carry = 0;
    		curr->next = new ListNode(sum);
    		curr = curr -> next;
    		l1 = l1 ? l1 -> next : nullptr;
    		l2 = l2 ? l2 -> next : nullptr;
    	}
    	if (carry == 1) curr->next = new ListNode(1);
    	return dummy->next;
    }
};
```