---
layout: post
title:  "169. Majority Element"
date: 2019-01-10 20:48:23 -0400
categories: articles
---
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:
```
Input: [3,2,3]
Output: 3
```
Example 2:
```
Input: [2,2,1,1,1,2,2]
Output: 2
```
# Function signature
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        
    }
};
```
# 题意
从一个array里面找到majority，majority是出现次数超过 n/2的那个数。
# 想法
这个题一上来看着还是挺简单的，但是需要用一个extra space。能不能不用？
排序？排了的话就成了nlogn了。
因为是一个无序数组。针对一个无序数组我们能做些什么？？
不可以用2二叉，不可以有sliding window。sliding window一般是寻找substring的。
所以我觉得只能用extra space来找了。
```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
    	unordered_map<int, int> records;
        int n = nums.size();
    	for (int i = 0; i < n; ++i){
            records[nums[i]]++;
    		int count = records[nums[i]];
    		if (count > n/2) return nums[i];
    	}
    	return 0;
    }
};

```
# 参考答案
```c++
// 哇塞！！！！太NB的算法啦！！ 这个叫什么？？神一样的想法！！
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int n = 0, c = 0;
        for (int i : nums) {
            if (i == c) {
                n++;
            } else if (n == 0) {
                c = i;
                n = 1;
            } else {
                n--;
            }
        }
        return c;
    }
};
```