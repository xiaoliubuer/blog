---
layout: post
title:  "287. Find the Duplicate Number"
date: 2018-10-09 19:43:23 -0400
categories: articles
---
Given an array nums containing n + 1 integers where each integer is between 1 and n (inclusive), prove that at least one duplicate number must exist. Assume that there is only one duplicate number, find the duplicate one.

Example 1:

Input: [1,3,4,2,2]
Output: 2
Example 2:

Input: [3,1,3,4,2]
Output: 3
Note:

You must not modify the array (assume the array is read only).
You must use only constant, O(1) extra space.
Your runtime complexity should be less than O(n2).
There is only one duplicate number in the array, but it could be repeated more than once.

思路1:
1. 很简单用一个set，扫一遍，存起来，有就有，没就没。O(N), space N(n)

思路2:
1. 用O(N), space O(1)
因为长度是一样的，如果没有重复的话每个值出现一次，但现在又重复啦。那就很好玩啦。
所以按道理，1 2 3 2 5 0，这个样子的话。就很有意思，怎么办呢？？ 应该是要能后扫到4，但是现在没有4。

这样，假设我们每个数组是门牌号，我们要按照这个顺利下显示的gate number去访问每个gate，访问过了就把它的门打开(-num[num[i]]), 所以按道理，如果没有重复的话，我们是不会遇到被打开的门的。但是如果有重复，我们回发现一定会遇到过去的时候已经被打开过了。

比如 0  1, 2, 2, 3, 4, 5, 我们第一次遇到2的时候，已经把门牌为2的变成-2了，当我们又看到让你去访问2的时候，发现2号门已经被访问过了。于是2这个门牌号就是重复的。我么i从0到n，按下标来便利。我们把 __num[ num[i] ]__ 上的元素变成它的 Negative。

```c++
//Answer:
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
		if (nums.size() == 0) return 0;
		for (int i = 0; i < nums.size(); ++i){
			int index = abs(nums[i])-1;
			if (nums[index] < 0)
            	return abs(index+1);
            	nums[index] = -nums[index];
		}
		return 0;
    }
};
```
```c++
//Accepted!!!
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size() - 1; i++){
            if (nums[i] == nums[i+1]) return nums[i];
        }
        return -1;
    }
};
```
```c++
// Accepted!!!!
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        for(int i = 0; i < nums.size(); i++){
            if (nums[abs(nums[i]) - 1] < 0 ) return abs(nums[i]);
            nums[abs(nums[i]) - 1] *= -1;
        }
        return -1;
    }
};
```
