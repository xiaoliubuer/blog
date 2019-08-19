---
layout: post
title:  "605. Can Place Flowers"
date: 2019-08-14 19:34:00 -0400
categories: articles
---	
Suppose you have a long flowerbed in which some of the plots are planted and some are not. However, flowers cannot be planted in adjacent plots - they would compete for water and both would die.

Given a flowerbed (represented as an array containing 0 and 1, where 0 means empty and 1 means not empty), and a number n, return if n new flowers can be planted in it without violating the no-adjacent-flowers rule.

Example 1:
```
Input: flowerbed = [1,0,0,0,1], n = 1
Output: True
```
Example 2:
```
Input: flowerbed = [1,0,0,0,1], n = 2
Output: False
```
Note:
```
The input array won't violate no-adjacent-flowers rule.
The input array size is in the range of [1, 20000].
n is a non-negative integer which won't exceed the input array size.
```
```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        //简单的corner case处理就是在第一个的前面和最后一个的后面都加一个0
        //就不用写特别多的if else了
        flowerbed.insert(flowerbed.begin(),0);
        flowerbed.push_back(0);
        for(int i = 1; i < flowerbed.size()-1; ++i)
        {
            if(flowerbed[i-1] + flowerbed[i] + flowerbed[i+1] == 0)
            {
                --n;
                ++i;
            }
                
        }
        return n <=0;
    }
};
```
```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        vector<int> L(flowerbed.size());
        for(int i=0;i<flowerbed.size();i++){
            if(flowerbed[i]==1){
                L[i]=1;
                if(i>0) L[i-1]=1;
                if(i<flowerbed.size()-1) L[i+1]=1;
            }
        }
        int s=0;
        for(int i=0;i<L.size();i++){
            if(L[i]==0){
                s++;
                if(i<L.size()-1) L[i+1]=1;
            }
        }        
        if(s>=n) return true;
        else return false;
    }
};
```
```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& nums, int n) {
        int cur = 0; int l = nums.size();
        for(int i = 0; i < nums.size(); ++i) {
            if(nums[i] == 0 && (i + 1 == l || nums[i + 1] == 0)) {
                cur++; i++;
            } else if(nums[i] == 1) {
                i++;
            }
        }
        return n <= cur;
    }
};
```
```c++
class Solution {
public:
    bool canPlaceFlowers(vector<int>& flowerbed, int n) {

        unsigned int count = 0;
        unsigned int size = flowerbed.size();
        if(size == 1 && flowerbed[0] == 0) {
            count = 1;
            return count >= n ? true:false;
            
        }
        for(int i = 0; i < size; i++){
            if(flowerbed[i] == 1){
                i++;
            }else {
                if((i-1) >=0 && (i+1) < size) {
                    if(flowerbed[i+1] == 0 && flowerbed[i-1] == 0){
                        count++;
                        i++;
                    } 
                }else if(i == 0 && flowerbed[i+1] == 0){
                    count++;
                    i++;
                }else if(i == size-1 && flowerbed[i-1] == 0){
                    count++;
                    i++;
                }
            }
        }
        return count >= n ? true:false;
        
    }
};
```