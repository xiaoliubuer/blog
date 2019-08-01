---
layout: post
title:  "398. Random Pick Index"
date: 2019-07-31 22:08:00 -0400
categories: articles
---
Given an array of integers with possible duplicates, randomly output the index of a given target number. You can assume that the given target number must exist in the array.

Note:
The array size can be very large. Solution that uses too much extra space will not pass the judge.

Example:
```
int[] nums = new int[] {1,2,3,3,3};
Solution solution = new Solution(nums);

// pick(3) should return either index 2, 3, or 4 randomly. Each index should have equal probability of returning.
solution.pick(3);

// pick(1) should return 0. Since in the array only nums[0] is equal to 1.
solution.pick(1);
```
```c++
class Solution {
public:
    Solution(vector<int>& nums) {
        m_nums = move(nums);
    }
    
    int pick(int target) {
        int n=0;
        int index = -1;
        for (int i=0; i<m_nums.size(); ++i){
            if (m_nums[i] != target) continue;
            ++n;
            if (rand() % n ==0) index = i;
            
        }
        return index;
            
    }
private:
    vector<int> m_nums;
};
```
```c++
class Solution {
public:
    Solution(vector<int> nums) {
        this->nums = nums;
    }
    
    int pick(int target) {
        int res = -1, count = 0;
        for(int i = 0; i < nums.size(); i++){
            if(nums[i] != target) continue;
            if(res == -1) res = i, count++;
            else{
                count++;
                if(rand() % count == 0) res = i;
            }
        }
        return res;
    }
    
private:
    vector<int>nums;
};
```
```c++
class Solution {
public:
    Solution(vector<int>& nums) {
        nums_ = &nums;
    }
    
    int pick(int target) {
        int count = 0;
        int result = -1;
        for (int i = 0; i < nums_->size(); ++i) {
            if (nums_->at(i) != target) continue;
            ++count;
            if (random() % count == 0) result = i;
        }
        return result;
    }
    
    vector<int>* nums_;
};
```
```c++
class Solution {
    vector<int>arr;
public:
    Solution(vector<int>& nums) {
        arr = nums;
        srand(time(NULL));
    }
    //SOLUCHAN STARTS HERE - DUN DUN DUN DUN
    int pick(int target) {
        vector<int>ans;
        for(int i=0;i<arr.size();i++){
            if(arr[i] == target)
                ans.push_back(i);
        }
        
        // cout<<rand()%ans.size()<<endl;
        return ans[rand()%ans.size()];
    }
};
```