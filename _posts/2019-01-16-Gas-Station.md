---
layout: post
title:  ". 134. Gas Station"
date: 2019-01-16 22:04:23 -0400
categories: articles
---
There are N gas stations along a circular route, where the amount of gas at station i is gas[i].

You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.

Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.

Note:

If there exists a solution, it is guaranteed to be unique.
Both input arrays are non-empty and have the same length.
Each element in the input arrays is a non-negative integer.
Example 1:
```
Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]

Output: 3

Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
```
Example 2:
```
Input: 
gas  = [2,3,4]
cost = [3,4,3]

Output: -1

Explanation:
You can't start at station 0 or 1, as there is not enough gas to travel to the next station.
Let's start at station 2 and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 0. Your tank = 4 - 3 + 2 = 3
Travel to station 1. Your tank = 3 - 3 + 3 = 3
You cannot travel back to station 2, as it requires 4 unit of gas but you only have 3.
Therefore, you can't travel around the circuit once no matter where you start.
```
# Function signature
```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        
    }
};
```
# 题意
你的车油箱无限大，有一条路线是一个环，在环上分布着不同的加油站。每个加油站储存着不等量的油，并写着到下一个加油站需要消耗多少油的提示。你只能走顺时针的路线。有没有可能从一个加油站开始，能回到起点。

# 想法
创建一个数组，来保存它现在还有多少油。

从一个加油站开始，初始化，为那个加油站的油，然后判断现在有的油 - 距离是不是够。如果够，就减去腰用的油。
然后到了下一个 加上下一个加油站的油，然后再判断。到了边界。如果
# 尝试解解
```c++
class Solution {
public:
	bool helper(vector<int> tank, int idx, vector<int>& gas, vector<int>& cost) {
		int len = gas.size();
		for (int i = 0; i < len; ++i){
			if ( idx >= len) idx %= len;
			tank[idx] += gas[idx];
			if ( tank[idx] < cost[idx]) return false;
			idx++;
		}
		return true;
	}
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    	if (gas.size() == 0 || cost.size() == 0 ) return 0;
    	vector<int> tank(gas.size(), 0);
    	for (int i = 0; i < gas.size(); ++i){
    		if (helper(tank, i, gas, cost)) return i;
    	}
    	return -1;
    }
};
```
# Shame answer
```c++
//Greedy?? How??
class Solution {
public:
int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
    int size = gas.size();
    int sum = 0;
    int res = 0;
    int total = 0;
    for( int i = 0; i < size; ++i ){
        sum += gas[i] - cost[i];
        if( sum < 0 ){
            total += sum;
            sum = 0;
            res = i + 1;
        }
    }
    total += sum;
    return total < 0 ? -1 : res;
}
};
```