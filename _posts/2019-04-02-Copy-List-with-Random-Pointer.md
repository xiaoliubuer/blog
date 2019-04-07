---
layout: post
title:  "138. Copy List with Random Pointer"
date: 2019-04-02 21:34:23 -0400
categories: articles
---
A linked list is given such that each node contains an additional random pointer which could point to any node in the list or null.

Return a deep copy of the list.

Example 1:
```
Input:
{"$id":"1","next":{"$id":"2","next":null,"random":{"$ref":"2"},"val":2},"random":{"$ref":"2"},"val":1}

Explanation:
Node 1's value is 1, both of its next and random pointer points to Node 2.
Node 2's value is 2, its next pointer points to null and its random pointer points to itself.
``` 

Note:

You must return the copy of the given head as a reference to the cloned list.


```c++
class Solution {
public:
    RandomListNode *copyRandomList(RandomListNode *head) {
    	RandomListNode* res = new RandomListNode(0);

    	unordered_map<RandomListNode*, RandomListNode*> my_map;

    	RandomListNode* temp = head;
    	RandomListNode* cur = res;
    	while( temp ){
    		cur->next = new RandomListNode(0);
    		cur->next->label = temp->label;
            my_map[temp] = cur->next;
    		cur = cur->next;
    		temp = temp->next;
    	}
    	temp = head;
    	cur  = res;
    	while( temp ){
    		cur->next->random = my_map[temp->random];
            temp = temp->next;
            cur  = cur->next;
    	}
    	return res->next;
    }
};
```