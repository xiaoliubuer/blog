---
layout: post
title:  "628. Maximum Product of Three Numbers"
date: 2019-08-15 08:42:00 -0400
categories: articles
---	

Given an integer array, find three numbers whose product is maximum and output the maximum product.

Example 1:
```
Input: [1,2,3]
Output: 6
```

Example 2:
```
Input: [1,2,3,4]
Output: 24
```

Note:
```
The length of the given array will be in range [3,104] and all elements are in the range [-1000, 1000].
Multiplication of any three numbers in the input won't exceed the range of 32-bit signed integer.
```

```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        ios_base::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
        int thirdL = INT_MIN, secondL = INT_MIN, L = INT_MIN, secondS = INT_MAX, S = INT_MAX;
        
        for (int num : nums) {
            if (num > thirdL) {
                if (num > L) {
                    thirdL = secondL;
                    secondL = L;
                    L = num;
                }
                else if (num > secondL) {
                    thirdL = secondL;
                    secondL = num;
                }
                else {
                    thirdL = num;
                }
            }
            
            if (num < secondS) {
                if (num < S) {
                    secondS = S;
                    S = num;
                }
                else {
                    secondS = num;
                }
            }
        }
        return max(L * secondL * thirdL, L * secondS * S);
    }
};
```

```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        int m1=INT_MIN, m2=INT_MIN, m3=INT_MIN, l1=INT_MAX, l2=INT_MAX;
        
        for (int i : nums){
            if (i > m1){
                m3 = m2;
                m2 = m1;
                m1 = i;
            }else if (i > m2){
                m3 = m2;
                m2 = i;
            }else if (i > m3){
                m3 = i;
            }
            
            if (i < l1){
                l2 = l1;
                l1 = i;
            }else if (i < l2){
                l2 = i;
            }
        }
        
        return max(m1*m2*m3, m1*l1*l2);
    }
};
```

```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
       
        
        
        int n=nums.size();
        
        //find max1,max2,max3
        //find min1,min2
        
        int max1=INT_MIN;
        int max2=INT_MIN;
        int max3=INT_MIN;
        int min1=INT_MAX;
        int min2=INT_MAX;
        
        for(int i=0;i<n;i++)
        {
            if(nums[i]>max3)
            {
                max1=max2;
                max2=max3;
                max3=nums[i];
            }
            else if(nums[i]>max2)
            {
                max1=max2;
                max2=nums[i];
            }
            else if(nums[i]>max1)
            {
                max1=nums[i];
            }
            
            if(nums[i]<min1)
            {
                min2=min1;
                min1=nums[i];
            }
            else if(nums[i]<min2)
            {
                min2=nums[i];
            }
            
        }
        
        return max(max1*max2*max3,min1*min2*max3);
        
        
        
    }
};
```
```c++
class Solution {
public:
    int maximumProduct(vector<int>& nums) {
        std::sort(nums.begin(), nums.end());
        int n = nums.size();
        return max(nums[0] * nums[1] * nums[n-1], nums[n-1] * nums[n-2] * nums[n-3]);
    }
};
```
```c++
class Solution {
public:
int maximumProduct(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    int n = nums.size();
    int temp1 = nums[n-1]*nums[n-2]*nums[n-3];
    int temp2 = nums[0]*nums[1]*nums[n-1];
    return temp1>temp2?temp1:temp2;
}
};
```