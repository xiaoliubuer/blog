---
layout: post
title:  "485. Max Consecutive Ones"
date: 2019-08-06 20:09:00 -0400
categories: articles
---
Given a binary array, find the maximum number of consecutive 1s in this array.

Example 1:
```
Input: [1,1,0,1,1,1]
Output: 3
Explanation: The first two digits or the last three digits are consecutive 1s.
    The maximum number of consecutive 1s is 3.
```
Note:
```
The input array will only contain 0 and 1.
The length of input array is a positive integer and will not exceed 10,000
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int counter=0,max=0;
        for(int i= 0 ; i<nums.size() ; i++)
        {
            if (nums[i] == 1) counter++;
            if ((nums[i] != 1 && counter != 0) || (i==nums.size()-1))
            {
                if(counter>max) max = counter;
                counter = 0; 
            }
        }
        return max;
    }
};
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
      int res=0;
      int sum=0;
        for(int num:nums)
        {
          sum=(sum+num)*num;
          res=max(res,sum);
        }
       return res;
    }
};
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int cnt = 0;
        int out = 0;
        for(int i=0;i<nums.size();i++){
            if(nums[i]==0){
                cnt = 0;
            }
            else{
                cnt+=1;
                out = max(out,cnt);
            }
        }
        return out;
    }
};
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        ios_base::sync_with_stdio(false);cin.tie(nullptr);
        int i = 0;
        int size = (int) nums.size();
        if(size == 0){return 0;}
        int counter = 0;
        int max = 0;
        
        for(int i = 0; i < size; ++i)
        {
            if(nums[i] != 0){counter++;}
            if(counter > max){max=counter;}
            if(nums[i]==0){
                counter=0;
                if(max>size - i){break;}
            }
            
        }
        
        return max;
    }
};
```