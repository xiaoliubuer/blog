---
layout: post
title:  "540. Single Element in a Sorted Array"
date: 2019-08-09 20:47:00 -0400
categories: articles
---
Given a sorted array consisting of only integers where every element appears exactly twice except for one element which appears exactly once. Find this single element that appears only once.

Example 1:
```
Input: [1,1,2,3,3,4,4,8,8]
Output: 2
```
Example 2:
```
Input: [3,3,7,7,10,11,11]
Output: 10
```

Note: Your solution should run in O(log n) time and O(1) space.

```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int ans = 0;
        for (int num:nums) {
            ans ^= num;
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while (l < r) {
            int mid = (l + r) / 2;
            if (mid % 2 == 1) mid--;
            if (nums[mid] == nums[mid + 1]) {
                l = mid + 2;
            }
            else r = mid;
        }
        return nums[l];
    }
};
```
```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& A) {
     int lo=0;
    int hi=A.size()-1;
        
        while(lo<=hi)
        {
            int mid=lo+(hi-lo)/2;
            if(mid==0 || mid==A.size()-1)
                return A[mid];
            else if(A[mid]!=A[mid-1] && A[mid]!=A[mid+1])
                return A[mid];
            
            else if(mid%2==0)
            {
                if(A[mid]==A[mid+1])
                    lo=mid+1;
                else
                    hi=mid-1;
                
            }
            else if(mid%2==1)
            {
                if(A[mid]==A[mid-1])
                    lo=mid+1;
                else
                    hi=mid-1;
            }
        }
        
        return A[lo];
    }
};
```
```c++
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        if(nums.size() == 1){
            return nums[0];
        }
        int left = 0, right = nums.size()-1;
        while(left < right){
            int mid = left + (right - left) / 2;
            if(nums[mid-1] != nums[mid] && nums[mid] != nums[mid+1]){
                return nums[mid];
            }
            if(mid % 2 == 0){
                if(nums[mid] == nums[mid+1]){
                    left = mid + 2;
                }else{
                    right = mid - 2;
                }
            }else{
                if(nums[mid] == nums[mid+1]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }
        }
        return nums[left];
    }
};
```