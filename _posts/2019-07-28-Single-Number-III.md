---
layout: post
title:  "260. Single Number III"
date: 2019-07-28 16:21:00 -0400
categories: articles
---

Given an array of numbers nums, in which exactly two elements appear only once and all the other elements appear exactly twice. Find the two elements that appear only once.

Example:
```
Input:  [1,2,1,3,2,5]
Output: [3,5]
```
Note:
```
The order of the result is not important. So in the above example, [5, 3] is also correct.
Your algorithm should run in linear runtime complexity. Could you implement it using only constant space complexity?
```

```c++
class Solution {
public:
vector<int> singleNumber(vector<int>& nums) {
    int aXorb = 0;  // the result of a xor b;
    for (auto item : nums) aXorb ^= item;
    int lastBit = (aXorb & (aXorb - 1)) ^ aXorb;  // the last bit that a diffs b
    int intA = 0, intB = 0;
    for (auto item : nums) {
        // based on the last bit, group the items into groupA(include a) and groupB
        if (item & lastBit) intA = intA ^ item;
        else intB = intB ^ item;
    }
    return vector<int>{intA, intB};   
}
};
```