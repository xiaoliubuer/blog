---
layout: post
title:  "218. The Skyline Problem"
date: 2019-07-27 19:00:00 -0400
categories: articles
---
A city's skyline is the outer contour of the silhouette formed by all the buildings in that city when viewed from a distance. Now suppose you are given the locations and height of all the buildings as shown on a cityscape photo (Figure A), write a program to output the skyline formed by these buildings collectively (Figure B).

Buildings  Skyline Contour
The geometric information of each building is represented by a triplet of integers [Li, Ri, Hi], where Li and Ri are the x coordinates of the left and right edge of the ith building, respectively, and Hi is its height. It is guaranteed that 0 ≤ Li, Ri ≤ INT_MAX, 0 < Hi ≤ INT_MAX, and Ri - Li > 0. You may assume all buildings are perfect rectangles grounded on an absolutely flat surface at height 0.

For instance, the dimensions of all buildings in Figure A are recorded as: [ [2 9 10], [3 7 15], [5 12 12], [15 20 10], [19 24 8] ] .

The output is a list of "key points" (red dots in Figure B) in the format of [ [x1,y1], [x2, y2], [x3, y3], ... ] that uniquely defines a skyline. A key point is the left endpoint of a horizontal line segment. Note that the last key point, where the rightmost building ends, is merely used to mark the termination of the skyline, and always has zero height. Also, the ground in between any two adjacent buildings should be considered part of the skyline contour.

For instance, the skyline in Figure B should be represented as:[ [2 10], [3 15], [7 12], [12 0], [15 10], [20 8], [24, 0] ].

Notes:

The number of buildings in any input list is guaranteed to be in the range [0, 10000].
The input list is already sorted in ascending order by the left x position Li.
The output list must be sorted by the x position.
There must be no consecutive horizontal lines of equal height in the output skyline. For instance, [...[2 3], [4 5], [7 5], [11 5], [12 7]...] is not acceptable; the three lines of height 5 should be merged into one in the final output as such: [...[2 3], [4 5], [12 7], ...]

```c++
class Solution {
public:
    struct Mycomp {
        bool operator()(vector<int>& v1, vector<int>& v2) {
            if(v1[2]<v2[2]) {
                return true;
            } else if(v1[2]==v2[2]) {
                return v1[1]<v2[1];
            } else {
                return false;
            }
        }  
    };
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        priority_queue<vector<int>,vector<vector<int>>,Mycomp> heap;
        int currloc=INT_MIN;
        int buildingnum = 0;
        vector<vector<int>> ret;
        while(buildingnum!=buildings.size() || (!heap.empty())) {
            if(buildingnum==buildings.size() || ((!heap.empty()) && heap.top()[1]<buildings[buildingnum][0])) {//first meet end of heap.top() rightwal
                currloc=heap.top()[1];
                while((!heap.empty())&&heap.top()[1]<=currloc) {
                    heap.pop(); // now heap.top() is still fresh
                }
                if(heap.empty()) {// now is ground
                    vector<int> vec = {currloc,0};
                    ret.push_back(vec);
                    // ret.push_back(make_pair(currloc,0));
                } else {//move to the lower building
                    vector<int> vec = {currloc,heap.top()[2]};
                    ret.push_back(vec);
                    // ret.push_back(make_pair(currloc,heap.top()[2])); // the uppermost building
                }
                
            } else {//first meet buildings[buildingnum] left wall
                currloc=buildings[buildingnum][0];
                while(buildingnum!=buildings.size() && buildings[buildingnum][0]==currloc) {       
                    heap.push(buildings[buildingnum]);
                    buildingnum++;
                }
                if(heap.empty() || ret.empty() || heap.top()[2]>ret.back()[1]){
                    vector<int> vec = {currloc,heap.top()[2]};
                    ret.push_back(vec);
                    // ret.push_back(make_pair(currloc,heap.top()[2]));// the uppermost building
                }   
            }
        }
        return ret;
    }
};
```
```c++
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {
        // [left, right, height]   
        // [1, 10, 10], [3, 7, 15], [5, 12, 12]
        //           ____________
        //          |            |
        //          |      ______|_____________       
        //      ____|_____|______|________     |
        //     |    |     |      |        |    |
        //     |    |     |      |        |    |
        // ____|____|_____|______|________|____|___   
        //     1    3     5      7        10   12
        
        // key point marked everytime the maximum height changes
        // -> use priority queue
        
        // break buildings to left and right edges seperately
        // use multiset (not set) to sort them by location
        // Pay Attention: consider the case [2,3,3],[3,5,3]
        //            ___ ______
        //           |   |      |
        //     ______|___|______|___________
        //           2   3      5
        // we don't want to mark a point at location 3
        // thus, we need to pay attention to the sorted order
        // if two edges have the same location, we let left edges come first
        auto cmp = [](pair<int, int> a, pair<int, int> b)
        {
            if(a.first == b.first)
                return a.second > b.second;
            else
                return a.first < b.first;
        };
        
        multiset<pair<int, int>, decltype(cmp)> edges(cmp);
        for(auto & ele : buildings)
        {
            edges.insert(make_pair(ele[0], ele[2]));
            // use negative number to represent it's a right edge
            edges.insert(make_pair(ele[1], -ele[2]));
        }
        
        // idea: left edge -> enqueue, right edge -> dequeue
        // but we can't dequeue a value from priority queue directly
        // even can't iterate on the priority queue
        // so we use a hashmap to store values that pending to dequeue
        priority_queue<int> pq;
        unordered_map<int, int> pending_delete;
        vector<vector<int>> ans;
        
        for(auto &e : edges)
        {
            int before = 0;
            if(!pq.empty())
                before = pq.top();
            
            if(e.second > 0)
                pq.push(e.second);
            else
            {
                pending_delete[-e.second]++;
             
                // always keep the top of pq is valid (discard pending ones)
                while(!pq.empty() && pending_delete[pq.top()])
                {
                    pending_delete[pq.top()]--;
                    pq.pop();
                }
            }
            
            int after = 0;
            if(!pq.empty())
                after = pq.top();
            
            // mark a point everytime the maximum height changes
            if(before != after)
                ans.push_back({e.first, after});
        }
        
        return ans;
    }
    /*********complexity: time - O(nlogn), space - O(n)**********/
};
```