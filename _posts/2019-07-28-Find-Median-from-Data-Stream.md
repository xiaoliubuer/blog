---
layout: post
title:  "295. Find Median from Data Stream"
date: 2019-07-28 20:12:00 -0400
categories: articles
---
Median is the middle value in an ordered integer list. If the size of the list is even, there is no middle value. So the median is the mean of the two middle value.

For example,
[2,3,4], the median is 3

[2,3], the median is (2 + 3) / 2 = 2.5

Design a data structure that supports the following two operations:

void addNum(int num) - Add a integer number from the data stream to the data structure.
double findMedian() - Return the median of all elements so far.
Example:
```
addNum(1)
addNum(2)
findMedian() -> 1.5
addNum(3) 
findMedian() -> 2
```
Follow up:

If all integer numbers from the stream are between 0 and 100, how would you optimize it?
If 99% of all integer numbers from the stream are between 0 and 100, how would you optimize it?


```c++
class MedianFinder {
public:
    /** initialize your data structure here. */
    MedianFinder() {

    }
    
    void addNum(int num) {
        maxHalf.push(num);
        minHalf.push(maxHalf.top());
        maxHalf.pop();
        
        if (minHalf.size() > maxHalf.size()){
            maxHalf.push(minHalf.top());
            minHalf.pop();
        }
    }
    
    double findMedian() {
        if (minHalf.size() == maxHalf.size()) return (double)(minHalf.top() + maxHalf.top())/2;
        else return (double) maxHalf.top();
        
    }
    
    priority_queue<int, vector<int>, less<int>> minHalf;
    priority_queue<int, vector<int>, greater<int>> maxHalf;
};
```