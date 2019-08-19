---
layout: post
title:  "594. Longest Harmonious Subsequence"
date: 2019-08-11 18:12:00 -0400
categories: articles
---	
We define a harmounious array as an array where the difference between its maximum value and its minimum value is exactly 1.

Now, given an integer array, you need to find the length of its longest harmonious subsequence among all its possible subsequences.

Example 1:
```
Input: [1,3,2,2,5,2,3,7]
Output: 5
```
Explanation: 
```
The longest harmonious subsequence is [3,2,2,2,3].
```
Note: The length of the input array will not exceed 20,000.
```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        if(nums.size()==0)
            return 0;
        int subseqlength = 0;
        sort(nums.begin(),nums.end());
        int a = 0;
        int i = 1;
        while(a<nums.size()-1){
            int sub = 1;
            while(i<nums.size() and nums[i++] == nums[a])
                sub++;
            if (nums[a] == nums[i-1]-1){
                int b = i-1;
                sub++;
                while(i<nums.size() and nums[i++]==nums[b])
                    sub++;
                subseqlength = max(subseqlength,sub);
                a = b;
                i = a + 1;
            }
            else
                a = i-1;
        }
        return subseqlength;
    }
};
```
```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        unordered_map<int, int> mp;
        int n, maxcnt = 0, cnt;
        unordered_map<int, int>::iterator it;
        for (int n : nums) ++mp[n];
        for (const auto& v : mp) {
            n = v.first;
            it = mp.find(n + 1);
            if (it == mp.end()) continue;
            cnt = v.second + it->second;
            if (cnt > maxcnt) maxcnt = cnt;
        }
        return maxcnt;
    }
};
```
```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        if(nums.empty()) return 0;
        
        int res = 0;
        sort(nums.begin(), nums.end());
        
        int start = 0;
        int nextStart = 0;
        for(int i = 1; i < nums.size(); ++i) {
            if(nums[i] - nums[start] > 1) {
                start = nextStart;
            }
            if(nums[i] - nums[start] == 1) {
                res = max(res, i - start + 1);
            }
            if(nums[i] != nums[i-1]) nextStart = i;
            
        }
        
        return res;
    }
};
```
```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        int longest = 0;
        unordered_map<int, int>m;
        for(int i = 0; i < nums.size(); i++){
            m[nums[i]]++;
        }
        for(auto i: m){
            auto index = i.first;
            if(m.count(i.first+1))
                longest = max(longest, i.second+m[i.first+1]);       
        }
        return longest;
    }
};
```