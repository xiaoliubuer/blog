---
layout: post
title:  "876. Middle of the Linked List"
date: 2019-01-27 14:45:23 -0400
categories: articles
---
Given a non-empty, singly linked list with head node head, return a middle node of linked list.

If there are two middle nodes, return the second middle node.

 

Example 1:
```
Input: [1,2,3,4,5]
Output: Node 3 from this list (Serialization: [3,4,5])
The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
Note that we returned a ListNode object ans, such that:
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
```
Example 2:
```
Input: [1,2,3,4,5,6]
Output: Node 4 from this list (Serialization: [4,5,6])
Since the list has two middle nodes with values 3 and 4, we return the second one.
```

Note:
```
The number of nodes in the given list will be between 1 and 100.
```
# Function signature
```c++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        
    }
};
```
# 题意
找到链表的中间的那个元素

# 想法
快慢指针

# 尝试解解
```c++
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        if (!head) return head;
        ListNode* slow = head, *fast = head;
        while (fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
};
```