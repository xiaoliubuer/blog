---
layout: post
title:  "281. Zigzag Iterator"
date: 2019-05-27 18:52:00 -0400
categories: articles
---
Given two 1d vectors, implement an iterator to return their elements alternately.

Example:
```
Input:
v1 = [1,2]
v2 = [3,4,5,6] 

Output: [1,3,2,4,5,6]
```
Explanation: By calling next repeatedly until hasNext returns false, 
             the order of elements returned by next should be: [1,3,2,4,5,6].
Follow up: What if you are given k 1d vectors? How well can your code be extended to such cases?

Clarification for the follow up question:
The "Zigzag" order is not clearly defined and is ambiguous for k > 2 cases. If "Zigzag" does not look right to you, replace "Zigzag" with "Cyclic". For example:
```
Input:
[1,2,3]
[4,5,6,7]
[8,9]

Output: [1,4,8,2,5,9,3,6,7].
```
```c++
class ZigzagIterator {
public:
    ZigzagIterator(vector<int>& v1, vector<int>& v2) {
        
        int i = 0, j = 0;
        while ( i < v1.size() || j < v2.size()) {
            if ( i < v1.size() ){
                container.push_back(v1[i++]);
            }
            if ( j < v2.size() ) {
                container.push_back(v2[j++]);
            }
        }
    }

    int next() {
        return container[idx++];
    }

    bool hasNext() {
        return idx < container.size();
    }
    vector<int> container;
    int idx = 0;
};
```