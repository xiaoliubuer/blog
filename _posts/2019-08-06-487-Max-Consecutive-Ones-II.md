---
layout: post
title:  "487. Max Consecutive Ones II"
date: 2019-08-06 20:14:00 -0400
categories: articles
---
Given a binary array, find the maximum number of consecutive 1s in this array if you can flip at most one 0.

Example 1:
```
Input: [1,0,1,1,0]
Output: 4
Explanation: Flip the first zero will get the the maximum number of consecutive 1s.
    After flipping, the maximum number of consecutive 1s is 4.
```
Note:
```
The input array will only contain 0 and 1.
The length of input array is a positive integer and will not exceed 10,000
```
Follow up:
```
What if the input numbers come in one by one as an infinite stream? In other words, you can't store all numbers coming from the stream as it's too large to hold in memory. Could you solve it efficiently?
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int count_1 = 0; int count_2 = 0;
        int answer = 0; bool has_zero = false;
        
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 0){
                count_2 = count_1;
                count_1 = 0;
                has_zero = true;
            } else {
                count_1++;
            }
            answer = max(answer, count_1 + count_2 + (has_zero == true));
        }
        
        return answer;
    }
};
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int res(0), start(0), end(0), sz(nums.size()), count(0);
        while(end < sz) {
            if(nums[end] == 0)
                ++count;
            while(count > 1) {
                if(nums[start] == 0)
                    --count;
                ++start;
            }
            res = max(res, end - start + 1);
            ++end;
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int findMaxConsecutiveOnes(vector<int>& nums) {
        int start=0;
        int maxConsecutive=0;
        int currZ=0;
        
        for(int i=0; i<nums.size(); i++){
            if(nums[i]==0) currZ+=1; //currZ+=1-A[i];
            
            while(currZ>1){
                if(nums[start]==0) currZ--;
                start++;
            }
            
            maxConsecutive = max(maxConsecutive, i-start+1);
        }
        
        return maxConsecutive; 
        
    }
};
```