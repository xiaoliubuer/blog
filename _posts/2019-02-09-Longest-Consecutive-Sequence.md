---
layout: post
title:  "128. Longest Consecutive Sequence"
date: 2019-02-09 20:15:23 -0400
categories: articles
---
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Your algorithm should run in O(n) complexity.

Example:
```
Input: [100, 4, 200, 1, 3, 2]
Output: 4
Explanation: The longest consecutive elements sequence is [1, 2, 3, 4]. Therefore its length is 4.
```
```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        
    }
};
```
# 题意
就是在一个array里面找到一个最长的连续的序列
# 想法
O(n)的时间复杂度，就很有意思。
怎么判断这个是连续的？确定是连续的
就是把所有的数存到一个set中，然后扫描。
```c++
//Accepted!!
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> myset(nums.begin(), nums.end());
        int res = 0;
        for (auto i : nums) {
            if (!myset.count(i-1)) {
                int j = 0;
                while (myset.find(i) != myset.end()) {
                    i++;
                    j++;
                    res = max(res, j);
                }
            }
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
  	unordered_set<int> myset(nums.begin(), nums.end());
  	int longestStreak = 0;
  	for (int i : myset){
  		if ( myset.find( i - 1 ) == myset.end() ){
  			int currentNum = i;
  			int currentStreak = 1;
  			while( myset.find(currentNum + 1) != myset.end()){
  				currentNum++;
  				currentStreak++;
  			}
  			longestStreak = max(longestStreak, currentStreak);
  		}
  	}
  	return longestStreak;
    }
};
```
```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> set;
        int answer = 0;
        int n = nums.size();
        
        for(int i = 0; i < n; i++)
            set.insert(nums[i]);
        
        for(int i = 0; i < n; i++){
            if(set.find(nums[i]-1) == set.end()){
                int j = nums[i];
                while(set.find(j) != set.end())
                    j++;
                answer = max(answer, j - nums[i]); 
            }
        }
        
        return answer;
    }
};
```
<!-- 100, 4, 200, 1, 3, 2 -->