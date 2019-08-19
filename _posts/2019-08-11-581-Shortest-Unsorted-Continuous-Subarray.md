---
layout: post
title:  "581. Shortest Unsorted Continuous Subarray"
date: 2019-08-11 17:39:00 -0400
categories: articles
---	
Given an integer array, you need to find one continuous subarray that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the shortest such subarray and output its length.

Example 1:
```
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
```
Explanation:
```
You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```
Note:
```
Then length of the input array is in range [1, 10,000].
The input array may contain duplicates, so ascending order here means <=.
```
```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        if(nums.empty()) return 0;
        
        vector<int> sortedNums(nums.begin(), nums.end());
        sort(sortedNums.begin(), sortedNums.end());
        
        int beg = INT_MAX;
        int end = INT_MAX;
        const int sz = nums.size();
        for(int i = 0; i < sz; ++i) {
            if(beg == INT_MAX && nums[i] != sortedNums[i]) {
                beg = i;
            }
            
            const int revIndex = sz - i - 1;
            if(end == INT_MAX && nums[revIndex] != sortedNums[revIndex]) {
                end = revIndex;
            }
        }
        
        return (beg == INT_MAX && end == INT_MAX) ? 0 : end - beg + 1;
    }
};
```

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int N = nums.size();
        vector<int> left_max(N, INT_MIN), right_min(N, INT_MAX); 
        left_max[0] = nums[0], right_min.back() = nums.back(); 
        for (int i = 1; i < N; ++i){
            left_max[i] = max(left_max[i-1], nums[i]); 
        }
        for (int i = N-2; i >= 0; --i){
            right_min[i] = min(right_min[i+1], nums[i]); 
        }
        int start = 0, end = N - 1; 
        for (; start + 1 < N && nums[start] <= right_min[start + 1]; ++start){}
        for (; end > 0 && left_max[end - 1] <= nums[end]; --end){}
        return end > start ? end - start + 1 : 0; 
    }
};
```

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> t(nums);
        sort(begin(t), end(t));
        int i = 0;
        for(; i < nums.size(); ++i) {
            if(nums[i] != t[i]) break;
        }
  
        if(i == nums.size()) return 0;
        
        int j = nums.size() - 1;
        for(; j >= 0; --j) {
            if(nums[j] != t[j]) break;
        }
        
        return j - i + 1;
    }
};
```
```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        vector<int> t(nums);
        sort(begin(t), end(t));
        int i = 0;
        for(; i < nums.size(); ++i) {
            if(nums[i] != t[i]) break;
        }
  
        if(i == nums.size()) return 0;
        
        int j = nums.size() - 1;
        for(; j >= 0; --j) {
            if(nums[j] != t[j]) break;
        }
        
        return j - i + 1;
    }
};
```
```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int res = 0;
        vector<int> temp = nums;
        std::sort(temp.begin(), temp.end());
        int left = 0;
        int right = nums.size()-1;
        while(left < right){
            if(temp[left] == nums[left]){
                left++;
            }
            if(temp[right] == nums[right]){
                right--;
            }
            
            if(left == right)
                return 0;
            
            int temp_res = right - left + 1;
            if (temp_res == res )
                break;
            else
                res = temp_res;
            
        }
        return res;
    }
};
```