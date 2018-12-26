---
layout: post
title:  "324. Wiggle Sort II"
date: 2018-10-09 18:08:23 -0400
categories: articles
---

Given an unsorted array nums, reorder it such that nums[0] < nums[1] > nums[2] < nums[3]....

Example 1:

Input: nums = [1, 5, 1, 1, 6, 4]
Output: One possible answer is [1, 4, 1, 5, 1, 6].
Example 2:

Input: nums = [1, 3, 2, 2, 3, 1]
Output: One possible answer is [2, 3, 1, 3, 1, 2].
Note:
You may assume all input has valid answer.

Follow Up:
Can you do it in O(n) time and/or in-place with O(1) extra space?

思路：
这道题怎么想呢？？

他一定要严格的小大小大。而且保证输入的数是一定能个至少生成一个这样的排序。
所以举个极端的例子就是1 1 1 2 2，要生成 1 2 1 2 1， 6699
那么如何保证一定能这样呢？
也就是：
现排序，然后把后一半往前移
不对
有一种特例，4556
```c++
//Answer:
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
        if (nums.size() <= 1 ) return;
        std::sort(nums.begin(), nums.end()); // TC = O(nlogn)
        vector<int> temp(nums.size(), -1); //SC = O(n)

        int left    = 1;
        int right   = nums.size()%2 == 0 ? nums.size() - 2 : nums.size() - 1;

        int i = 0, j = nums.size()-1;
        while( i < j ){
            temp[left] = nums[j];
            temp[right] = nums[i];
            i++;
            j--;
            left  += 2;
            right -= 2;
        }
        if ( nums.size()%2 !=0 ) temp[0] = nums[nums.size()/2];
        nums = temp;
    }
};
```