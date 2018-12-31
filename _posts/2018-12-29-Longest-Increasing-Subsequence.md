---
layout: post
title:  "300. Longest Increasing Subsequence"
date: 2018-12-29 06:52:23 -0400
categories: articles
---
Given an unsorted array of integers, find the length of longest increasing subsequence.

Example:
```
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```
Note:

There may be more than one LIS combination, it is only necessary for you to return the length.
Your algorithm should run in O(n2) complexity.
Follow up: Could you improve it to O(n log n) time complexity?

# 题意
给一个未排序的integer数组，找到最长的增长子序列

# 思路
一般听到最大，最小之类的话，就说明是DP的题了。这道题可以用滑动窗口，什么意思呢？就是两个指针，left, right, left指针负责收尾，right指针负责拓宽领域。

所以right指针就是不断在向右移动，增加窗口的大小，而left指针在不符合增加窗口的条件下就用来缩小窗口。

1. Function signature
```c++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        
    }
};
```
2. Corner case:
```c++
if (nums.size() == 0 ) return 0;
int n = nums.size();
```

3. Two pointers
```c++
int left = 0, right = 0;
int res = INT_MIN;
```

4. Move pointers, calculate windowsize
```c++
while (right < n) {
	int temp = right - left;
	res = max(res, temp);

	if ( nums[left] > nums[right])
		left = right;
	right++;
}
```

5. Return res
```c++
return res;
```

# 错误答案
```c++
// 注意，这个subsequence可以是不连续的！！
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if (nums.size() == 0 ) return 0;
        int n = nums.size();
        int left = 0, right = 0;
        int res = 0;
        while ( right < n ) {
            int temp = right - left;
            res = max( res, temp);
            if ( nums[left] > nums[right]) left = right;
            right++;
        }
        return res;
    }
};
```

# 参考答案
```c++
//DP 好好品味这个答案
class Solution {
 public:
 int lengthOfLIS(vector<int>& nums) {
    int size=nums.size(), max=0;
    if(size==1 || size==0)return size;
    vector<int>dist(size, 1);

    for(int i=1; i< size; i++){
        for(int j=0; j < i;j++ ){
            if(nums[j] < nums[i] && dist[i] < dist[j] + 1 ) 
            	dist[i] = dist[j] + 1;
        }
        if(dist[i] > max)
        	max = dist[i];
    }
        return max;
  }
};
```	