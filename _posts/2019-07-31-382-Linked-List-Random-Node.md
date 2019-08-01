---
layout: post
title:  "382. Linked List Random Node"
date: 2019-07-31 20:55:00 -0400
categories: articles
---

Given a singly linked list, return a random node's value from the linked list. Each node must have the same probability of being chosen.

Follow up:
What if the linked list is extremely large and its length is unknown to you? Could you solve this efficiently without using extra space?

Example:
```
// Init a singly linked list [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);
```
// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
solution.getRandom();

```c++
class Solution {
    vector<int> n;
public:
    /** @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node. */
    Solution(ListNode* head) {
        while (head) {
            n.push_back(head->val);
            head = head->next;
        }
    }
    
    /** Returns a random node's value. */
    int getRandom() {
        return n[rand()%n.size()];
    }
};
```
```c++
class Solution {
public:

    ListNode* u;
    Solution(ListNode* head) {
        u = head;
    }
    
    int getRandom() {
        int res, len = 1;
        ListNode* v = u;
        while(v){
            if(rand() % len == 0){
                res = v->val;
            }
            len++;
            v = v->next;
        }
        return res;
    }
};
```