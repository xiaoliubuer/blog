---
layout: post
title:  "630. Course Schedule III"
date: 2019-08-15 08:49:00 -0400
categories: articles
---	
There are n different online courses numbered from 1 to n. Each course has some duration(course length) t and closed on dth day. A course should be taken continuously for t days and must be finished before or on the dth day. You will start at the 1st day.

Given n online courses represented by pairs (t,d), your task is to find the maximal number of courses that can be taken.

Example:
```
Input: [[100, 200], [200, 1300], [1000, 1250], [2000, 3200]]
Output: 3
```
Explanation: 
```
There're totally 4 courses, but you can take 3 courses at most:
First, take the 1st course, it costs 100 days so you will finish it on the 100th day, and ready to take the next course on the 101st day.
Second, take the 3rd course, it costs 1000 days so you will finish it on the 1100th day, and ready to take the next course on the 1101st day. 
Third, take the 2nd course, it costs 200 days so you will finish it on the 1300th day. 
The 4th course cannot be taken now, since you will finish it on the 3300th day, which exceeds the closed date.
```
Note:
```
The integer 1 <= d, t, n <= 10,000.
You can't take two courses simultaneously.
```
```c++
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(courses.begin(), courses.end(), 
             [](vector<int> &a, vector<int> &b) { return a[1] < b[1]; });
        priority_queue<int> pq;
        long time = 0;
        for (auto &course : courses) {
            if (time + course[0] <= course[1]) {
                time += course[0];
                pq.push(course[0]);
            } else if (!pq.empty() && pq.top() > course[0]) {
                time += (course[0] - pq.top());
                pq.pop();
                pq.push(course[0]);
            }
        }
        return pq.size();
    }
};
```
```c++
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        sort(begin(courses), end(courses), [](const auto& a, const auto& b) {
          return a[1] < b[1];
        });
        priority_queue<int> q;
        int d = 0;
        for (const auto& c: courses) {
          if (d + c[0] <= c[1]) {        
            d += c[0];
            q.push(c[0]);
          } else if (q.size() && q.top() > c[0]) {
            d += c[0] - q.top(); q.pop();
            q.push(c[0]);
          }
        }
        return q.size();
    }
};
```
```c++
class Solution {
public:
    int scheduleCourse(vector<vector<int>>& courses) {
        auto lambda = [] (vector<int> const &a, vector<int> const &b)
        {
           return a[1] < b[1] or (a[1] == b[1] and a[0] < b[0]);
        };
        sort(courses.begin(), courses.end(), lambda);
        priority_queue<int> heap;
        int time = 0;
        for(int i = 0; i < courses.size(); i++) {
            if(courses[i][0] + time <= courses[i][1]) {
                time += courses[i][0];
                heap.emplace(courses[i][0]);
            } else if(!heap.empty()) {
                if(heap.top() > courses[i][0]) {
                    time += courses[i][0] - heap.top();
                    heap.pop();
                    heap.emplace(courses[i][0]);
                }
            }
        }
        return (int)heap.size();
    }
};
```
```c++
class Solution {
public:
      using course = std::vector<int> ;
      int scheduleCourse(std::vector<course>& courses) {
             //std::cout << "start: " << courses << std::endl;
             std::sort(std::begin(courses), std::end(courses),
                       [] (course& left, course& right){
                   return left[1]<right[1];
             });
             //std::cout << "sort: " << courses << std::endl;
             int days = 0;
             std::priority_queue <int> duration;
             for(const auto& _course: courses) {
                   duration.push(_course[0]);
                   days += _course[0];
                   if (days > _course[1]) {
                         days -= duration.top();
                         duration.pop();
                   }
             }
            return duration.size();
      }
};
```
```c++
class Solution {
public:
      using course = std::vector<int> ;
      int scheduleCourse(std::vector<course>& courses) {
             //std::cout << "start: " << courses << std::endl;
             std::sort(std::begin(courses), std::end(courses),
                       [] (course& left, course& right){
                   return left[1]<right[1];
             });
             //std::cout << "sort: " << courses << std::endl;
             int days = 0;
             std::priority_queue <int> duration;
             for(const auto& _course: courses) {
                   duration.push(_course[0]);
                   days += _course[0];
                   if (days > _course[1]) {
                         days -= duration.top();
                         duration.pop();
                   }
             }
            return duration.size();
      }
};
```