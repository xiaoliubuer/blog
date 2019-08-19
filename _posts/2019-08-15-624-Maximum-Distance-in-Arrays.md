---
layout: post
title:  "624. Maximum Distance in Arrays"
date: 2019-08-15 08:25:00 -0400
categories: articles
---	

Given m arrays, and each array is sorted in ascending order. Now you can pick up two integers from two different arrays (each array picks one) and calculate the distance. We define the distance between two integers a and b to be their absolute difference ( a ~ b ). Your task is to find the maximum distance.

Example 1:
```
Input: 
[ [ 1,2,3],
 [4,5],
 [1,2,3] ]
Output: 4
```
Explanation: 
```
One way to reach the maximum distance 4 is to pick 1 in the first or third array and pick 5 in the second array.
Note:
Each given array will have at least 1 number. There will be at least two non-empty arrays.
The total number of the integers in all the m arrays will be in the range of [2, 10000].
The integers in the m arrays will be in the range of [-10000, 10000].
```
```c++
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        int minVal = arrays[0].front();
        int maxVal = arrays[0].back();
        int result = INT_MIN;
        for (int j = 1; j < arrays.size(); j++) if (!arrays[j].empty()) {
            int newMin = arrays[j].front();
            int newMax = arrays[j].back();
            result = max(result, max(newMax - minVal, maxVal - newMin));
            minVal = min(newMin, minVal);
            maxVal = max(newMax, maxVal);
        }
        return result;
    }
};
```
```c++
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        int maxd = 0;
        int n = arrays.size();
        if(n <= 1)
            return maxd;
        int pmin = arrays[0][0];
        int pmax = arrays[0][arrays[0].size() - 1];
        for(int i = 1; i < n; i++)
        {
            int l = arrays[i][0];
            int r = arrays[i][arrays[i].size() - 1];
            maxd = max(maxd, pmax - l);
            maxd = max(maxd, r - pmin);
            pmin = min(pmin, l);
            pmax = max(pmax, r);
        }
        return maxd;
    }
};
```
```c++
class Solution {
public:
  int maxDistance(std::vector<std::vector<int>>& arrays) {
    int min_index{0}, min{arrays[0].front()};
    int max_index{0}, max{arrays[0].back()};
    int max_distance = 0;
    for (int i = 1; i < arrays.size(); ++i) {
      if (i != min_index) {
        max_distance = std::max(max_distance, (arrays[i].back() - min));
      }
      if (arrays[i].front() < min) {
        min = arrays[i].front(), min_index = i;
      }

      if (i != max_index) {
        max_distance = std::max(max_distance, (max - arrays[i].front()));
      }
      if (arrays[i].back() > max) {
        max = arrays[i].back(), max_index = i;
      }
    }
    return max_distance;
  }
};
```
```c++
class Solution {
public:
    int maxDistance(vector<vector<int>>& arrays) {
        
        int smallest = arrays[0][0];
        int biggest = arrays[0][arrays[0].size()-1];
        int maxDiff = 0;
        
        for (int i=1; i < arrays.size(); i++){
            
            int curr_smallest = arrays[i][0];
            int curr_biggest = arrays[i][arrays[i].size()-1];
            int curr_maxDiff = max( abs(biggest - curr_smallest), abs(curr_biggest - smallest)  );
            
            smallest = min ( smallest, curr_smallest );
            biggest = max ( biggest, curr_biggest );
            maxDiff = max( maxDiff, curr_maxDiff );
        }
        
        return maxDiff;
    }
};
```