---
layout: post
title:  "225. Implement Stack using Queues"
date: 2019-07-27 19:28:00 -0400
categories: articles
---

Implement the following operations of a stack using queues.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
empty() -- Return whether the stack is empty.
Example:
```
MyStack stack = new MyStack();

stack.push(1);
stack.push(2);  
stack.top();   // returns 2
stack.pop();   // returns 2
stack.empty(); // returns false
```
Notes:

You must use only standard operations of a queue -- which means only push to back, peek/pop from front, size, and is empty operations are valid.
Depending on your language, queue may not be supported natively. You may simulate a queue by using a list or deque (double-ended queue), as long as you use only standard operations of a queue.
You may assume that all operations are valid (for example, no pop or top operations will be called on an empty stack).


```c++
class MyStack {
public:
    /** Initialize your data structure here. */
    queue<int> q1;
    queue<int> q2;
    MyStack(){
        q1 = queue<int>();
        q2 = queue<int>();
    };
    
    /** Push element x onto stack. */
    void push(int x) {
 			q1.push(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
    	if (q1.empty()) return 0;
       	int res = 0;
       	while(!q1.empty()){
       		if ( q1.size() == 1) res = q1.front();
       		if ( q1.size() > 1) q2.push(q1.front());
        	q1.pop();
        }
        q1 = q2;
        while(!q2.empty()){
        	q2.pop();
        }
        return res;
    }
    
    /** Get the top element. */
    int top() {
    	if (q1.empty()) return 0;
    	int res = 0;
       	while(!q1.empty()){
       		if ( q1.size() == 1) res = q1.front();
            q2.push(q1.front());
        	q1.pop();
        }
        q1 = q2;
        while(!q2.empty()){
        	q2.pop();
        }
        return res;
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return q1.empty();
    }
};
```