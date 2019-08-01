---
layout: post
title:  "365. Water and Jug Problem"
date: 2019-07-31 20:15:00 -0400
categories: articles
---

You are given two jugs with capacities x and y litres. There is an infinite amount of water supply available. You need to determine whether it is possible to measure exactly z litres using these two jugs.

If z liters of water is measurable, you must have z liters of water contained within one or both buckets by the end.

Operations allowed:

Fill any of the jugs completely with water.
Empty any of the jugs.
Pour water from one jug into another till the other jug is completely full or the first jug itself is empty.
Example 1: (From the famous "Die Hard" example)
```
Input: x = 3, y = 5, z = 4
Output: True
```
Example 2:
```
Input: x = 2, y = 6, z = 5
Output: False
```
```c++
class Solution {
public:
int gcd(int x, int y) {
	return y==0?x:gcd(y, x % y);
}

bool canMeasureWater(int x, int y, int z) {
	return z==0 || (x+y>=z && z % gcd(x,y)==0);
}
};
```

```c++
class Solution {
public:
    bool canMeasureWater(int x, int y, int z) {
        if(z == 0) return true;
        if(x + y < z)
            return false;
        return !(z % gcd(x, y));
    }
};
```
```c++
class Solution {
public:
    bool canMeasureWater(int x, int y, int z) {
        if (x+y == z)
            return true;
        if (x+y < z)
            return false;
        if (x > y) {
            int temp = x;
            x = y;
            y = temp;
        }
        int volume = 0;
        while (1) {
            if (volume < x)
                volume += y;
            else
                volume -= x;
            if (volume == z)
                return true;
            if (volume == 0)
                return false;
        }
    }
};
```