---
layout: post
title:  "All sort algorithms"
date: 2019-01-02 08:22:23 -0400
categories: articles
---

# Bubble Sort
```c++

void BubbleSort(){

}


```
# Insert Sort
```c++
void InsertSort(){

}
```

# Shell Sort
```c++
```



# Quick Sort
```c++

void QuickSort(){

}
```

# Merge Sort
```c++
void MergeSort(){

}
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
```