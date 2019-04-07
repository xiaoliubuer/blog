---
layout: post
title:  "391. Number of Airplanes in the Sky"
date: 2019-01-09 20:26:23 -0400
categories: articles
---
# Lintcode
https://www.lintcode.com/problem/number-of-airplanes-in-the-sky/description

Description
Given an interval list which are flying and landing time of the flight. How many airplanes are on the sky at most?

Example
For interval list
```
[
  (1,10),
  (2,3),
  (5,8),
  (4,7)
]
Return 3
```
# Finction signature
```c++
class Solution {
public:
    /**
     * @param airplanes: An interval array
     * @return: Count of airplanes are in the sky.
     */
    int countOfAirplanes(vector<Interval> &airplanes) {
        // write your code here
    }
};
```
# 题意
给一个list interval是起飞和降落的时间，问有多少飞机最多同时在天上。
# 想法
最多飞机在天上是啥意思？意思就是这堆intervals里面最多有几个overlap的intervals。
那么就是怎么找到最多的overlap的intervals呢？？

所以如果把这些intervals进行以start排序，然后有一种机制对这些时间点进行扫描。如果扫到start，flying++，扫到end，flying--，就在这++，--的过程中去找到最大的那个。flying.

那这就有个问题，如果模拟一个扫描仪对这些书进行扫描呢？？

要一个probity_queue?，用来存放end
每扫到一个interval，必须保证当前这个start < queue.top, 否则就queue.pop()

# 尝试答案
```c++
class Solution {
public:
    int countOfAirplanes(vector<Interval> &airplanes) {
    	if (airplanes.size() < 2) return airplanes.size();
    	int count = 0;
    	int res = 0;
    	priority_queue<int, vector<int>, greater<int> > landing;// 定义 最小堆，就是顶头是最小值
    	//priority_queue<int, vector<int>, less<int> > landing;// 定义 最大堆， 顶头最大值

    	sort(airplanes.begin(), airplanes.end(), [](Interval& a, Interval& b){
    	    return a.start < b.start;
    	});// 必须要排序， 不排序的话就错了
    	
    	for( int i = 0; i < airplanes.size(); i++ ){
    		while(!landing.empty() && landing.top() <= airplanes[i].start ){
    			landing.pop();
    			count--;
    		}
    		landing.push(airplanes[i].end);
    		count++;
    		res = max(res, count);
    	}
    	return res;
    }
};
```
```c++
// Wrong Answer!!!!
class Solution {
public:
    /**
     * @param airplanes: An interval array
     * @return: Count of airplanes are in the sky.
     */
    bool isOverlap(Interval A, Interval B){
        if ( A.start >= B.end || A.end <= B.start ) return false;
        else
        return true;
    }
    int countOfAirplanes(vector<Interval> &airplanes) {
        // write your code here
        int res = 0;
        if (airplanes.size() < 2) return airplanes.size();
        for (int i = 1; i < airplanes.size() - 1; i++){
            int count = 1;
            for (int j = i + 1; j < airplanes.size(); j++ ){
                if (isOverlap(airplanes[i], airplanes[j])) count++;
            }
            
            for ( int k = 0; k < i; k++ ){
                if(isOverlap(airplanes[i], airplanes[k])) count++;
            }
            res = max(res, count);
        }
        return res;
    }
};
```
```c++
// Accepted!!!
class Solution {
public:
    /**
     * @param airplanes: An interval array
     * @return: Count of airplanes are in the sky.
     */
    int countOfAirplanes(vector<Interval> &airplanes) {
        // write your code here
        if ( airplanes.size() < 2) return airplanes.size();
        int res = 0;
        int last = 0;
        unordered_map<int, int> mymap;
        for (auto i : airplanes) {
            mymap[i.start]++;
            mymap[i.end]--;
            last = max(last, max(i.start, i.end));
        }
        int count = 0;
        for (int i = 0; i <= last; i++){
            count += mymap[i];
            res = max(res, count);
        }
        return res;
    }
};
```
```c++
// Accepted!!! Beat 100%
class Solution {
public:
    int countOfAirplanes(vector<Interval> &airplanes) {
        // write your code here
        if ( airplanes.size() < 2) return airplanes.size();
        vector<int> start, end;
        for (auto i : airplanes) {
            start.push_back(i.start);
            end.push_back(i.end);
        }
        sort(start.begin(), start.end());
        sort(end.begin(), end.end());
        int count = 0;
        int res = 0;
        for (int i = 0, j = 0; j < end.size() && i < start.size();){
            if ( start[i] < end[j]) {
                count++;
                i++;
            }
            else{
                count--;
                j++;
            }
            res = max(res, count);
        }
        return res;
    }
};
```
# 参考答案
```c++
/* 基本思想，把start和end都存到vector里面。
然后给两个vector都排序
这就有了一个start的vector, 表示都是那个点有飞机起飞。
然后有一个end的vector，表示都有哪个点有飞机降落。

然后呢？？

然后就一个循环， 但有两个指针，一个指的是start vector，一个指的是end vector，然后。如果start指针小于end指针指的值，start指针一直++，然后更新res，否则end指针++，res--。
*/
class Solution {
public:
    int countOfAirplanes(vector<Interval> &airplanes) {
        int res = 0;
        vector<int> start_time;
        vector<int> end_time;
        for (int i = 0; i < airplanes.size(); i++){
                start_time.push_back(airplanes[i].start);
                end_time.push_back(airplanes[i].end);
        }
        sort(start_time.begin(), start_time.end());
        sort(end_time.begin(), end_time.end());
        int idx_end = 0;
        for (int i = 0; i < airplanes.size(); i++){
            if (start_time[i] >= end_time[idx_end]){
                res--;
                idx_end++;
            }
            res++;
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int countOfAirplanes(vector<Interval> &airplanes) {
        if (airplanes.size() < 2) return airplanes.size();
        int count = 0;
        int res = 0;
        priority_queue<int, vector<int>, greater<int> > landing;
        sort(airplanes.begin(), airplanes.end(), [](Interval& a, Interval& b){
            return a.start < b.start;
        });
        
        for( int i = 0; i < airplanes.size(); i++ ){
            while(!landing.empty() && landing.top() <= airplanes[i].start ){
                landing.pop();
                count--;
            }
            landing.push(airplanes[i].end);
            count++;
            res = max(res, count);
        }
        return res;
    }
};
```