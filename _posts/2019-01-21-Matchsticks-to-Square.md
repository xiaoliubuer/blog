---
layout: post
title:  "473. Matchsticks to Square"
date: 2019-01-21 14:45:23 -0400
categories: articles
---
Remember the story of Little Match Girl? By now, you know exactly what matchsticks the little match girl has, please find out a way you can make one square by using up all those matchsticks. You should not break any stick, but you can link them up, and each matchstick must be used exactly one time.

Your input will be several matchsticks the girl has, represented with their stick length. Your output will either be true or false, to represent whether you could make one square using all the matchsticks the little match girl has.

Example 1:
```
Input: [1,1,2,2,2]
Output: true

Explanation: You can form a square with length 2, one side of the square came two sticks with length 1.
```
Example 2:
```
Input: [3,3,3,3,4]
Output: false

Explanation: You cannot find a way to form a square with all the matchsticks.
```
Note:
The length sum of the given matchsticks is in the range of 0 to 10^9.
The length of the given matchstick array will not exceed 15.

# Function signature
```c++
class Solution {
public:
    bool makesquare(vector<int>& nums) {
        
    }
};
```
# 题意
卖火柴的小女孩？能不能用所有的火柴围成一个square？要用完所有额火柴，而且火柴不可以被折断。
# 想法
首先总和一定可以被4整除。然后能个形成4组那么长的边的组合。
# 尝试解解
```c++
class Solution {
public:
	bool helper(vector<int>& nums, int currLen){
		if ( currLen == 0) return true;
        
    	for (int i = 0; i < nums.size(); ++i){
    		if ( nums[i] > currLen) return false;
    		if ( nums[i] > 0 && currLen - nums[i] >= 0 ){
    			nums[i] = -nums[i];
    			if ( helper(nums, currLen - abs(nums[i])) ) return true;
    			nums[i] = abs(nums[i]);
    		}
    	}
    	return false;
	}

    bool makesquare(vector<int>& nums) {
    	int sum = 0;
    	for (int i : nums) sum += i;
        sort(nums.begin(), nums.end(), [](int& a, int& b){ // This is important
        	// In case the case is : 1 2 3 4 5 6 7 8 9 10 1 12 13 14 15, will return false.
            return a > b;
        });
    	if ( sum % 4 != 0 || nums.empty() ) return false;
    	for (int i = 0; i < 3; ++i){
    		if ( helper( nums, sum/4 ) == false ) return false;
    	}
    	return true;
    }
};
```