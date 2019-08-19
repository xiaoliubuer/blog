---
layout: post
title:  "658. Find K Closest Elements"
date: 2019-08-18 16:03:00 -0400
categories: articles
---
Given a sorted array, two integers k and x, find the k closest elements to x in the array. The result should also be sorted in ascending order. If there is a tie, the smaller elements are always preferred.

Example 1:
```
Input: [1,2,3,4,5], k=4, x=3
Output: [1,2,3,4]
```
Example 2:
```
Input: [1,2,3,4,5], k=4, x=-1
Output: [1,2,3,4]
```
Note:
```
The value k is positive and will always be smaller than the length of the sorted array.
Length of the given array is positive and will not exceed 104
Absolute value of elements in the array and x will not exceed 104
```
```c++
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        auto n = arr.size();
        auto lo = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
        int l = lo-1, r = lo;
        for (; k; --k) {
            if (l >=0 && (r==n || x-arr[l] <= arr[r]-x)) {
                --l;
            } else {
                ++r;
            }
        }
        return vector<int>(arr.begin()+l+1, arr.begin()+r);
    }
};
```
```c++
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        auto n = arr.size();
        auto lo = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
        int l = lo-1, r = lo;
        for (; k; --k) {
            if (l >=0 && (r==n || x-arr[l] <= arr[r]-x)) {
                --l;
            } else {
                ++r;
            }
        }
        return vector<int>(arr.begin()+l+1, arr.begin()+r);
    }
};
```
```c++

class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        auto n = arr.size();
        auto lo = lower_bound(arr.begin(), arr.end(), x) - arr.begin();
        auto up = upper_bound(arr.begin(), arr.end(), x) - arr.begin();

        if (arr[lo] == x) {
            --lo;
        }
        while ((lo >= 0 && up < n) && (up-lo-1)<k) {
            if (x-arr[lo] <= arr[up]-x) {
                --lo;
            } else {
                ++up;
            }
        }
        if (up-lo-1==k) {
            return vector<int>(arr.begin()+lo+1, arr.begin()+up);
        } else {
            if (lo<0) {
                return vector<int>(arr.begin(), arr.begin() + k);
            } else {
                return vector<int>(arr.end() - k, arr.end());
            }
        }

    }
};
```
```c++
class Solution {
public:
    vector<int> findClosestElements(vector<int>& arr, int k, int x) {
        auto less_lambda = [&x](const int lhs, const int rhs) -> bool {
            int diff_lhs = abs(lhs - x);
            int diff_rhs = abs(rhs - x);
            if (diff_lhs < diff_rhs)
                return true;
            else if (diff_lhs > diff_rhs)
                return false;
            else {
                if (lhs < rhs)
                    return true;
                else
                    return false;
            }
        };
        auto greater_lambda = [&less_lambda](const int &lhs, const int &rhs) -> bool {
            return less_lambda(rhs, lhs);
        };
        if (k >= arr.size())
            return arr;
        priority_queue<int, vector<int>, decltype(greater_lambda)> pq(greater_lambda);
        for (int y : arr)
            pq.push(y);
        vector<int> result;
        for (int i = 0; i < k; ++i) {
            result.push_back(pq.top());
            pq.pop();
        }
        sort(result.begin(), result.end());
        return result;
    }
};
```