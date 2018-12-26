---
layout: post
title:  "154. Find Minimum in Rotated Sorted Array II"
date: 2018-10-27 10:40:23 -0400
categories: articles
---

Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.

(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).

Find the minimum element.

The array may contain duplicates.

Example 1:

Input: [1,3,5]
Output: 1
Example 2:

Input: [2,2,2,0,1]
Output: 0

# 思路:
其实就是binary search， 关键是找最小，而且有相等的情况。
那么怎么办呢？？

我们就把min设置成第一个元素。
然后在array中间找一个值。
比较
如果 min > mid?
如果 min < mid?
如果 min = mid?
**所以呢？？那是要往左呢？还是往右呢？如何判定？？**

所以就是如果没办法比较的话，就只能线性移动来进行缩小范围.

1. 如果中间🀄️大于右边👉，那么右半段一定会更细小！！？？？？？为什么？？
对！！那么一定是小的被挤到右边了。 找小的？去右边～～
否则，小的一定在左边👈！！！？？？？为什么？？(mid < left)
因为, 想一下就知道了！！
如果相等。那么就把右边往左移一位啦～～～
2. 为什么一定要right ⬅️移，而不是left ➡️移呢？？
3. 要清晰的弄清楚是 left = mid + 1 ? 还是 right - 1? 还是怎样，保证这个改变是安全的。

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
  		int left = 0;
  		int right = nums.size() - 1;

  		if (nums.size() == 0) return 0;

  		while( left < right ){
  			int mid = left + (right - left)/2;

  			if ( nums[mid] > nums[right]){  // mid > right 
  				left = mid + 1;
  			}// 这里left = mid + 1 非常安全

  			else if( nums[mid] < nums[right]){
  				right = mid;
  			}// 如果在这里 right = mid - 1, 不安全
  			else{
  				right--;
  			}
  		}
  		return nums[right];
    }
};
```

答案2:
```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
  		int left = 0;
  		int right = nums.size() - 1;

  		if (nums.size() == 0) return 0;

  		while( left < right ){
  			int mid = left + (right - left)/2;
            if( nums[mid] < nums[left]){
  				right = mid;
  			}
            else if ( nums[mid] > nums[right]){
  				left = mid+1;
  			}
  			else{
  				right--;
  			}
  		}
  		return nums[right];
    }
};
```


所以，最后，逻辑是什么？？

你的目的就是要让 left 或者 right 根据 mid 进行↔️移动。
那么如何才能让左右↔️进行安全🔐的移动呢？？？

那就 假设确认一个 mid，
OK，mid定了，那么， 你要确保left指针，和right指针都能安全的进行移动。
现在，假定有了一个mid，那么我如何让left进行一个安全的向➡️移动呢？？？
只要有一种情况可以完完全全保证让left向右移动，那就可以了。那就是如下:
因为大前提:
```c++
	mid = left + (right - left)/2;
```
```c++
if ( nums[mid] > nums[right]){  // mid > right
	left = mid + 1;
}// 这里left = mid + 1 非常安全
```
仔细思考只有这一种情况。

那么如何让right向左⬅️移动呢？？
```c++
if( nums[mid] < nums[right]){
	right = mid;
}// 如果在这里 right = mid - 1, 不安全
```
或者
```c++
if( nums[mid] < nums[left]){
	right = mid;
}
```
仔细思考～为什么？？

这里非常重要！！！！

**3.最后是return哪个呢？？**
 
nums[left]? nums[right]? or nums[mid]?
Why？？？
