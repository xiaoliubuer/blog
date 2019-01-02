---
layout: post
title:  "* 23. Merge k Sorted Lists"
date: 2019-01-02 07:48:23 -0400
categories: articles
---
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.

Example:
```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```
# Function signature
```c++
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        
    }
};
```
# 题意
合并k个排好序的list，并分析复杂度。

# Thinking

# Shame answer
```c++
// Merge sort
class Solution {
public:
     ListNode* mergeKLists(vector<ListNode*>& lists) {
        return partition(lists, 0, lists.size()-1);
    }
    
    ListNode* partition(vector<ListNode*>& lists, int start, int end){
        if(start == end){
            return lists[start];
        }
        
        if(start < end){
            int mid = (end+start)/2;
            
            ListNode* l1 = partition(lists, start, mid);
            ListNode* l2 = partition(lists, mid+1, end);
            return merge(l1, l2);
        }
        
        return NULL;
    }
    
    ListNode* merge(ListNode* l1, ListNode* l2){
        if(!l1) return l2;
        if(!l2) return l1;
        
        if(l1->val <= l2->val){
            l1->next = merge(l1->next, l2);
            return l1;
        }else{
            l2->next = merge(l1, l2->next);
            return l2;
        }
    }
};
```
```c++
// priority_queue
class Solution {
public:
	struct Compare {
		bool operator() (const ListNode* m1, const ListNode* m2) { 
		return m1->val > m2->val;
		}
	};
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.empty()) return nullptr;
		priority_queue<ListNode*, vector<ListNode*>, Compare> node_seq; 
		for( auto i : lists){
			if(i)
				node_seq.push(i);
		}
		ListNode* new_head = new ListNode(0);
		ListNode* temp = new_head;
		while(!node_seq.empty()){
			ListNode* cur_top = node_seq.top();
			temp->next = cur_top;
			node_seq.pop();
			
			if (cur_top->next){
				node_seq.push(cur_top->next);
			}
			temp = temp->next;
		}
		return new_head->next;
    }
};
```