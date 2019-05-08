---
layout: post
title:  "56. Merge Intervals"
date: 2019-04-29 23:19:00 -0400
categories: articles
---
Given a collection of intervals, merge all overlapping intervals.

Example 1:
```
Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].
```
Example 2:
```
Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considered overlapping.
```
# Function signature
```c++
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        
    }
};
```
# 题意
给了一个pair的list，如果它们相交了，就合并起来。
# 想法
现排序？为什么先排序？？比如以左边界去排序。
最初的想法就是，把每个pair和现在res里面所有的pair进行对比，如果它们可以merge，那就merge，否则就push进去。
```c++
// Do I need this?
sort(intervals.begin(), intervals.end(), [](Interval& a, Interval& b){
	return a.first > b.first;
})
```
# 尝试答案
```c++
//Accepted!!
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        sort(intervals.begin(), intervals.end(), [](Interval& A, Interval& B){
            return A.start < B.start;
        });
        if ( intervals.size() < 2) return intervals;
        vector<Interval> res;
        Interval curr = intervals[0];
        for (int i = 1; i < intervals.size(); i++){
            if (intervals[i].start <= curr.end) 
                curr.end = max(curr.end, intervals[i].end);
            else{
                res.push_back(curr);
                curr = intervals[i];
            }
        }
        res.push_back(curr);
        return res;
    }
};
```
```c++
// Accepted!!
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        unordered_map<int,int> st, ed;
        int low = 100, high = -100;
        for ( auto i : intervals){
            st[i[0]]++;
            ed[i[1]]++;
            low = min(low, i[0]);
            high= max(high,i[1]);
        }
        vector<vector<int>> res;
        int bal = 0;
        vector<int> temp;
        for (int i = low; i <= high; i++) {
            if (st[i] > 0) {
                bal += st[i];
                if ( temp.empty() ) temp.push_back(i);
            }
            if (ed[i] > 0) {
                bal -= ed[i];
                if ( bal == 0 ){
                    temp.push_back(i);
                    res.push_back(temp);
                    temp.clear();
                }
            }
        }
        return res;
    }
};
```
```c++
// Accepted! Use set. Bad performance.
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        map<int,int> st, ed;
        set<int> pos;
        int low = 100, high = -100;
        for ( auto i : intervals){
            st[i[0]]++;
            ed[i[1]]++;
            pos.emplace(i[0]);
            pos.emplace(i[1]);
        }
        vector<vector<int>> res;
        int bal = 0;
        vector<int> temp;
        for (auto it = pos.begin(); it != pos.end(); it++){
            if (st[*it] > 0) {
                bal += st[*it];
                if ( temp.empty() ) temp.push_back(*it);
            }
            if (ed[*it] > 0) {
                bal -= ed[*it];
                if ( bal == 0 ){
                    temp.push_back(*it);
                    res.push_back(temp);
                    temp.clear();
                }
            }
        }
        return res;
    }
};
```
```c++
// Accepted!! 
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        vector<int> st, ed;
        for ( auto i : intervals){
            st.push_back(i[0]);
            ed.push_back(i[1]);
        }
        sort(st.begin(), st.end());
        sort(ed.begin(), ed.end());
        vector<vector<int>> res;
        if (intervals.empty()) return res;
        int bal = 0;
        vector<int> temp;
        for (int i = 0, j = 0; i < st.size() && j < ed.size();){
            if (st[i] <= ed[j] ){
                if ( temp.empty() ) {
                    temp.push_back(st[i]);
                }
                bal++;
                i++;
            }
            else{
                if ( bal > 0 ) bal--;
                if ( bal == 0) {
                    temp.push_back(ed[j]);
                    res.push_back(temp);
                    temp.clear();
                }
                j++;
            }
        }
        if ( temp.size() == 1 ) temp.push_back(ed.back());
        res.push_back(temp);
        return res;
    }
};
```
```c++
//这个应该是work的
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
    	vector<Interval> res;
    	if (intervals.size() == 0) return res;
    	res.push_back(intervals[0]);
    	for (int i = 1; i < intervals.size(); i++){
    		for(int j = 0; j < res.size(); j++) {
    			if (res[j].start > intervals[i].end || res[j].end < intervals[i].start){
    				res[j].start = min(res[j].start, intervals[i].start);
    				res[j].end   = max(res[j].end, intervals[i].end);
    			}
    			else
    				res.push_back(intervals[i]);
    		}
    	}
    	return res;
    }
};
```
但是，因为这个是没有sort，那么在遍历的时候你就不知道到底是要往前merge，还是往后merge，为了往前merge，我们就要用two FOR loops。但是如果我们进行了排序，按照第一个边界进行了排序，那么我们就只需要管end的值了，就不需要顾及start的值是大是小了。所以，如果代码里面加了sort的话。

```c++
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
    	vector<Interval> res;
        if (intervals.size() == 0) return res; // 注意处理
    	sort(intervals.begin(), intervals.end(), [](Interval& a, Interval& b){
    		return a.start < b.start;
    	});
    	res.push_back(intervals[0]);
    	for (int i = 1; i < intervals.size(); i++){
    		if (intervals[i].start <= res.back().end){ // 注意处理 a.end == b.start 的情况
    			res.back().end = max(intervals[i].end, res.back().end);
    		}
    		else
    			res.push_back(intervals[i]);
    	}
        return res;
    }
};
```
# 参考答案
```c++
class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        if (intervals.size() == 0) return intervals;
        vector<Interval> res;
        sort(intervals.begin(), intervals.end(), [](Interval a, Interval b){
            return a.start < b.start;
        });
        res.push_back(intervals[0]);
        for (int i = 1; i < intervals.size(); i++){
            Interval back = res.back();
            if ( back.end >= intervals[i].start){
                back.end = max(back.end, intervals[i].end);
                res[res.size() - 1] = back;
            }
            else
                res.push_back(intervals[i]);
        }
        return res;
    }
};
```
