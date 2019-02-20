---
layout: post
title:  "264. Ugly Number II"
date: 2019-01-27 15:06:23 -0400
categories: articles
---
Write a program to find the n-th ugly number.

Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. 

Example:

Input: n = 10
Output: 12
Explanation: 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
Note:  

1 is typically treated as an ugly number.
n does not exceed 1690.
# Function signature
```c++
class Solution {
public:
	bool isUglyNumber(int n){

	}

    int nthUglyNumber(int n) {
        
    }
};
```
# 题意

# Shame answer
```c++
class Solution {
public:
    int nthUglyNumber(int n) {
	if (n < 1)
		return 0;

	vector<int> nums;
	nums.push_back(1);
	int indexA = 0, indexB = 0, indexC = 0;
	// 1, 2, 3, 5
	while (nums.size() < n)
	{
		int temp = min(min(nums[indexA] * 2, nums[indexB] * 3), nums[indexC] * 5);
		if (temp == nums[indexA] * 2) indexA++;
		if (temp == nums[indexB] * 3) indexB++;
		if (temp == nums[indexC] * 5) indexC++;
		nums.push_back(temp);
	}

	return nums[n - 1];
    }
};
```