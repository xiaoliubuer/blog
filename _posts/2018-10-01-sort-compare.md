---
layout: post
title:  "Sort and Compare, queue, priority"
date: 2018-01-01 20:46:23 -0400
categories: articles
---
# Sort
1. Example1:
```c++
sort(intervals.begin(), intervals.end(), [](Interval& interval1, Interval& interval2){
	return interval1.start < interval2.start;
});
```

2. Example2:
```c++
sort(es.begin(), es.end(), [](pair<int, int> a, pair<int, int> b){
	return a.first < b.first || (a.first == b.first && a.second > b.second);
});
```
3. Example3:
```c++
		sort(allocation.begin(), allocation.end(), [](pair<int, int>& a, pair<int, int>& b){
			int distance1 = a.first * a.first + a.second * a.second;
			int distance2 = b.first * b.first + b.second * b.second;
			return distance2 < distance1;
		});
```

# Compare
1. Example1:
```c++
PriorityQueue<vector<int>> pq = new PriorityQueue<vector<int>>(
	n,
	new Comparator<vector<int>>(){
		public int compare (vector<int> e1,vector<int> e2){
			return e1.get(0)*e1.get(0) + e1.get(1)*e1.get(1) - e2.get(0)*e2.get(0) - e2.get(1)*e2.get(1);
		}
	});
```
2. Example2:
```c++
struct Compare{
		int getDistance(Point a, Point b) {
		    return (a.x - b.x) * (a.x - b.x) + (a.y - b.y) * (a.y - b.y);
		}
		operator(const Point& a, const Point& b){
           int diff = getDistance(b, global_origin) - getDistance(a, global_origin);
           if (diff == 0) diff = b.x - a.x;
           if (diff == 0) diff = b.y - a.y;
           return diff;
		}
	};
queue<Point, vector<Point>, Compare> pq;
```
3. Example3:
```c++
struct MovieCompare {
		bool operator() (const Movie& m1, const Movie& m2) { 
		return m1.rating > m2.rating;
		}
	} movieCompare;
priority_queue<Movie, vector<Movie>, MovieCompare> min_heap;  // 默认为最小对，root为最小值
```
4. Example4:
```c++
priority_queue<int, vector<int>, less<int>>    myqueue; // Max heap, root is the largest
priority_queue<int, vector<int>, greater<int>> myqueue; // Min heap, root is the smallest
```


5. priority_queue<pair, vector<pair>, [](pair& A, pair& B){ A.first < B.first}> myqueue;