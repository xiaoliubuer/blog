---
layout: post
title:  "319. Bulb Switcher"
date: 2019-07-30 19:28:00 -0400
categories: articles
---

There are n bulbs that are initially off. You first turn on all the bulbs. Then, you turn off every second bulb. On the third round, you toggle every third bulb (turning on if it's off or turning off if it's on). For the i-th round, you toggle every i bulb. For the n-th round, you only toggle the last bulb. Find how many bulbs are on after n rounds.

Example:
```
Input: 3
Output: 1 
```
Explanation: 
```
At first, the three bulbs are [off, off, off].
After first round, the three bulbs are [on, on, on].
After second round, the three bulbs are [on, off, on].
After third round, the three bulbs are [on, off, off]. 
```
So you should return 1, because there is only one bulb is on.
```c++
class Solution {
public:
    /*以6为例，因子有1 2 3 6
    1 off->on
    2 on->off
    3 off->on
    6 on->off
    所以，因数个数决定最后结果

    还不如 2的所有倍数、3的所有倍数、4的所有倍数？？
    */
    int bulbSwitch(int n){
        int count=1;
        int index=1;
        while(index<=n){
            count++;
            index=count*count;
        }
        return count-1;
    }
};
```

```c++
class Solution {
public:
int bulbSwitch(int n) {
    int counts = 0;
    for (int i=1; i*i<=n; ++i) ++ counts;    
    return counts;
}
};
```