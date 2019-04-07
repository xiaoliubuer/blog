---
layout: post
title:  "163. Missing Ranges"
date: 2019-04-01 08:12:23 -0400
categories: articles
---
Given a sorted integer array nums, where the range of elements are in the inclusive range [lower, upper], return its missing ranges.

Example:
```
Input: nums = [0, 1, 3, 50, 75], lower = 0 and upper = 99,
Output: ["2", "4->49", "51->74", "76->99"]
```
```c++
// Not pass all, but pass few.
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        vector<string> res;
        if (nums.size() == 0 || lower > upper) return res;
        for ( int i = 0; i < nums.size(); i++ ){            
            if ( nums[i] > lower && nums[i] - lower > 1){
                res.push_back(to_string(lower + 1));
                if ( lower + 2 == nums[i]){
                    lower = nums[i];
                }
                else{
                    res[res.size() - 1] += "->";
                    string s = to_string(nums[i] - 1);
                    res[res.size() - 1] += s;
                    lower = nums[i];
                }
            }
            else{
                lower = nums[i];
            }
            if( nums[i] >= upper){
                if ( (double)(nums[nums.size() - 1] + 1) < upper){
                    res[res.size() - 1] += "->";
                    res[res.size() - 1] += to_string(upper);
                }
                break;
            }
        }
        if ( nums[nums.size() - 1] + 1 < upper){
                    res.push_back(to_string(nums[nums.size()-1] +1));
                    res[res.size() - 1] += "->";
                    res[res.size() - 1] += to_string(upper);
                }
        return res;
    }
};
```

```c++
// The same problem did not pass.
// [2147483647]
// 0
// 2147483647
//Line 13: Char 18: runtime error: signed integer overflow: 2147483647 - -1 cannot be represented in type 'int' (solution.cpp)

class Solution {
public:
    string get_range(int start, int end)
    {
        return start==end? to_string(start) : to_string(start)+"->"+to_string(end);
    }
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        vector<string> result;
        int pre = lower-1;
        for(int i =0; i <= nums.size(); i++)
        {
           int cur = (i==nums.size()? upper+1:nums[i]);
           if(cur-pre>=2)
            result.push_back(get_range(pre+1,cur-1));
            pre = cur;
        }
        return result;
    }
};
```