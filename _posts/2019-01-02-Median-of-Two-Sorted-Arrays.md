---
layout: post
title:  "* 4. Median of Two Sorted Arrays"
date: 2019-01-02 07:00:23 -0400
categories: articles
---
There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

You may assume nums1 and nums2 cannot be both empty.

Example 1:
```
nums1 = [1, 3]
nums2 = [2]

The median is 2.0
```
Example 2:
```
nums1 = [1, 2]
nums2 = [3, 4]

The median is (2 + 3)/2 = 2.5
```

# Function signature
```c++
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        
    }
};
```
# 题意
两个排好序的array nums1 和 nums2，size m 和 n。找到这两个array的中位数。时间复杂度为O(log(m+n)).
# 想法
中位数就排序后最中间的那个数，如果是even个数，那么就是中间两个数的sum/2，那就是把所有都存在一个vector里面，然后给它排序，然后找到最中间的一个或两个。O（nlog(m+n)）

如何做到log(m+n)?看上去有困难. 说白了就是binary search 这两个, 找到两个array的最中间的那个或那两个。

所以如何实现呢？？
# 看答案
```c++
// 能把这个做出来的那绝对是神人
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    	int m = nums1.size(); // 保存各个array的长度
    	int n = nums2.size(); 
    	if ( m > n ){ //If m > n, switch
    		vector<int> temp = nums1;
    		nums1 = nums2;
    		nums2 = temp; 

    		m ^= n;  
    		n ^= m;   
    		m ^= n;   
    	}
    	int iMin = 0, iMax = m, halfLen = (m+n+1)/2;

    	while (iMin <= iMax){
    		int i = (iMin + iMax)/2;
    		int j = halfLen - i;
    		if ( i < iMax && nums2[j-1] > nums1[i]){
    			iMin = i + 1;
    		}
    		else if ( i > iMin && nums1[i-1] > nums2[j]){
    			iMax = i - 1;
    		}
    		else{
    			int maxLeft = 0;
    			if ( i == 0 ){ maxLeft = nums2[j-1]; }
    			else if( j == 0 ) { maxLeft = nums1[i-1]; }
    			else { maxLeft = max(nums1[i-1], nums2[j-1]);}
    			if (( m + n ) % 2 == 1) { return maxLeft; }

    			int minRight = 0;
    			if ( i == m ){ minRight = nums2[j]; }
    			else if ( j == n ){ minRight = nums1[i]; }
    			else { minRight = min(nums2[j], nums1[i]); }

    			return (maxLeft + minRight) / 2.0;
    		}
    	}
    	return 0.0;
    }
};
```