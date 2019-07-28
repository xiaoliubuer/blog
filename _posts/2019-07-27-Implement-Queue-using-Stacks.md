---
layout: post
title:  "232. Implement Queue using Stacks"
date: 2019-07-27 19:46:00 -0400
categories: articles
---

Implement the following operations of a queue using stacks.
```
push(x) -- Push element x to the back of queue.
pop() -- Removes the element from in front of queue.
peek() -- Get the front element.
empty() -- Return whether the queue is empty.
```
Example:

MyQueue queue = new MyQueue();
```
queue.push(1);
queue.push(2);  
queue.peek();  // returns 1
queue.pop();   // returns 1
queue.empty(); // returns false
```
Notes:

You must use only standard operations of a stack -- which means only push to top, peek/pop from top, size, and is empty operations are valid.
Depending on your language, stack may not be supported natively. You may simulate a stack by using a list or deque (double-ended queue), as long as you use only standard operations of a stack.
You may assume that all operations are valid (for example, no pop or peek operations will be called on an empty queue).


```c++
class MyQueue {
public:
    /** Initialize your data structure here. */
    stack<int> s1;
    stack<int> s2;

    MyQueue() {
     s1 = stack<int>();
     s2 = stack<int>();   
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
      s1.push(x);
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        int res;
        if (s1.empty()) return 0;
        
        

        while(!s1.empty()){
        	if (s1.size() == 1) res = s1.top();
        	if (s1.size() > 1 ) s2.push(s1.top());
        	s1.pop();
        }
        
        while(!s2.empty()) {
            s1.push(s2.top());
            s2.pop();    
        }
        return res;
    }
    
    /** Get the front element. */
    int peek() {
        int res;
        if (s1.empty()) return 0;

        while(!s1.empty()){
        	if (s1.size() == 1) res = s1.top();
        	s2.push(s1.top());
        	s1.pop();
        }
        while(!s2.empty()) {
            s1.push(s2.top());
            s2.pop();    
        }
        return res;

    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty();
    }
};
```