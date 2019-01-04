---
layout: post
title:  "* Sort - Merge Sort"
date: 2019-01-02 08:22:23 -0400
categories: articles
---
```
Example:

4 6 7 3 4 5 2 4 5 34 5 65 
```

```c++
//Merge sort

void Merge(vector<int>& nums, int left, int mid, int right){
	
}

void partation(vector<int>& nums, int left, int right){
	if ( left >= right ) return;
	int mid = left + (right - left)/2;
	partation(nums, left, mid);
	partation(nums, mid+1, right);
	Merge(nums, left, mid, right);

}
void MergeSort(vector<int>& nums){

}