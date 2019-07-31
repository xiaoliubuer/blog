---
layout: post
title:  "346. Moving Average from Data Stream"
date: 2019-07-30 20:28:00 -0400
categories: articles
---
Given a stream of integers and a window size, calculate the moving average of all integers in the sliding window.
Example:
```
MovingAverage m = new MovingAverage(3);
m.next(1) = 1
m.next(10) = (1 + 10) / 2
m.next(3) = (1 + 10 + 3) / 3
m.next(5) = (10 + 3 + 5) / 3
```
```c++
class MovingAverage {
public:
    /** Initialize your data structure here. */
    queue<int> nums;
    int w_size = 0;
    int sum = 0;
    MovingAverage(int size) {
        w_size = size;
    }
    
    double next(int val) {
        sum += val;
        nums.push(val);
        if(nums.size() > w_size){
            int f = nums.front();
            nums.pop();
            sum -= f;
        }
        cout << "sum: " << sum << " size: " << nums.size() <<'\n';
        double res = sum/(double)nums.size();
        return res;
    }
};
```