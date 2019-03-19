---
layout: post
title:  "341. Flatten Nested List Iterator"
date: 2019-03-17 17:58:23 -0400
categories: articles
---
Given a nested list of integers, implement an iterator to flatten it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

Example 1:

Input: [[1,1],2,[1,1]]
Output: [1,1,2,1,1]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,1,2,1,1].
Example 2:

Input: [1,[4,[6]]]
Output: [1,4,6]
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,4,6].

```c++
// Accepted!
class NestedIterator {
public:
    
    void helper(vector<NestedInteger> &nestedList) {
        for (int i = 0; i < nestedList.size(); i++) {
            if (nestedList[i].isInteger())
                myqueue.push(nestedList[i].getInteger());
            else
                helper(nestedList[i].getList());
        }
        
    }
    NestedIterator(vector<NestedInteger> &nestedList) {
        helper(nestedList);
    }

    int next() {
        int front = myqueue.front();
        myqueue.pop();
        return front;
    }

    bool hasNext() {
        return !myqueue.empty();
    }
    
    queue<int> myqueue;
};
```