---
layout: post
title:  "215. Kth Largest Element in an Array"
date: 2019-05-07 21:37:00 -0400
categories: articles
---
Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

Example 1:
```
Input: [3,2,1,5,6,4] and k = 2
Output: 5
```
Example 2:
```
Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
```
Note: 
You may assume k is always valid, 1 ≤ k ≤ array's length.
```c++
class Solution {
public:
  int quick_select(vector<int>& nums, int start, int end, int k) {
    if (start == end) return nums[start];
    int i = start, j = end, pivot = nums[(i + j) / 2];
    while (i <= j) {
      while (i <= j && nums[i] > pivot) i++;
      while (i <= j && nums[j] < pivot) j--;
      if (i <= j) swap(nums[i++], nums[j--]);
    }
    if (start + k - 1 <= j)
      return quick_select(nums, start, j, k);
    if (start + k - 1 >= i)
      return quick_select(nums, i, end, k - (i - start));
    return nums[j + 1];
  }
  

  int findKthLargest(vector<int>& nums, int k) {
    return quick_select(nums, 0, nums.size() - 1, k);
  }
};
```
```c++
class Solution {
public:
int findKthLargest(vector<int>& nums, int k) {
        int n = nums.size();
        if (n==1) return nums[0];

        int big = nums[0], small = big;
        
        vector<int> left;
        vector<int> right;

        for (int i = 1; i < n; i++) { // Find the big number and the small number
            if (nums[i] > big)
                big = nums[i];

            if (nums[i] < small) 
                small = nums[i];
        }

        const double mid = (big + small) / 2.0; // use the medium as partition pivot

        for (int i = 0; i < n; i++) { 
            if (nums[i] <= mid)
                left.push_back(nums[i]);
            else
                right.push_back(nums[i]);
        }
        if (left.size() == n) return nums[0]; // all the elements are equal.
        if (left.size() >= n-k+1)
            return findKthLargest(left, k-right.size());
        else 
            return findKthLargest(right, k);
    }
};
```
```c++
// O(NlogN) O(N)
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int> myqueue;
        for (int i  = 0; i < nums.size(); i++ ) myqueue.push(nums[i]);
        while ( k > 1 ) {
            myqueue.pop();
            k--;
        }
        return myqueue.top();
    }
};
```