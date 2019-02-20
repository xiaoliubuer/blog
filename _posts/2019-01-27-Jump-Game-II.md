---
layout: post
title:  ". 45. Jump Game II"
date: 2019-01-27 09:45:23 -0400
categories: articles
---
Given an array of non-negative integers, you are initially positioned at the first index of the array.

Each element in the array represents your maximum jump length at that position.

Your goal is to reach the last index in the minimum number of jumps.

Example:
```
Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2.
    Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
Note:
```
You can assume that you can always reach the last index.
```
# Function signature
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        
    }
};
```
# 题意
这次返回的是最少的跳跃次数
# 想法
用DP来标记每次跳到的次数
# 尝试解解
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        int res = 0, n = nums.size();
        if ( n == 0 ) return 0;
        vector<int> dp(n, 100000);
        dp[0] = 0;
        for (int i = 0; i < n-1; i++){
            int scope = nums[i];
            int j = i + scope >= n - 1 ? n - 1 : i + scope;
            while (j >= i){
                dp[j] = min(dp[j], dp[i]+1);
                j--;
            }
        }
        return dp[n-1];
    }
};
```
```c++
// 思路就是有机会就往后延伸你的end reach的地方，一旦延伸到超过 n - 1的地方，那就return steps。
class Solution {
public:
    int jump(vector<int>& nums) {
        int steps = 0, n = nums.size(), end = 0, ext_end;
        if ( n == 0 ) return 0;
        end = nums[0];
        for ( int i = 0; i <= end; ++i ){
            steps++;
            end = i + nums[i];
            while ( i < end ){
                int new_end = i + nums[i];
                if ( new_end >= n - 1) return steps;
                ext_end = max( new_end, max( ext_end, end));
                i++;
            }
            end = ext_end;
        }
        return -1;   
    }
};
```

# 参考答案
```c++
class Solution {
public:
    int jump(vector<int>& nums) {
        if (nums.size() <= 1) return 0;
        int curr = 0;
        int end = nums.size() - 1;
        int step = 0;
        while (curr + nums[curr] < end)
        {
            int max = 0;
            for (int i = 1; i <= nums[curr]; ++i)
            {
                if (nums[curr + i] + i > nums[curr + max] + max)
                {
                    max = i;
                }
            }
            curr += max;
            ++step;
        }

        return ++step;
    }
};

// BFS
class Solution {
public:
    int jump(vector<int>& nums) {
        int n = nums.size(), step = 0, start = 0, end = 0;
        while (end < n - 1) { // end表示当前最远能够reach到的范围
            step++; 
            int maxend = end + 1;
            for (int i = start; i <= end; i++) {
                if (i + nums[i] >= n - 1) return step;
                maxend = max(maxend, i + nums[i]); // 这一句非常重要！
            }
            start = end + 1;
            end = maxend;
        }
        return step;
    }
};
```