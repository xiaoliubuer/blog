---
layout: post
title:  "659. Split Array into Consecutive Subsequences"
date: 2019-08-18 16:11:00 -0400
categories: articles
---
Given an array nums sorted in ascending order, return true if and only if you can split it into 1 or more subsequences such that each subsequence consists of consecutive integers and has length at least 3.

Example 1:
```
Input: [1,2,3,3,4,5]
Output: True
```
Explanation:
```
You can split them into two consecutive subsequences : 
1, 2, 3
3, 4, 5
```
Example 2:
```
Input: [1,2,3,3,4,4,5,5]
Output: True
```
Explanation:
```
You can split them into two consecutive subsequences : 
1, 2, 3, 4, 5
3, 4, 5
```
Example 3:
```
Input: [1,2,3,4,4,5]
Output: False
```

Constraints:

1 <= nums.length <= 10000

```c++
class Solution {
public:
    bool isPossible(std::vector<int>& nums) {
        int pre = INT_MIN, p1 = 0, p2 = 0, p3 = 0;
        int cur = 0, cnt = 0, c1 = 0, c2 = 0, c3 = 0;

        for (int i = 0; i < nums.size(); pre = cur, p1 = c1, p2 = c2, p3 = c3) {
            for (cur = nums[i], cnt = 0; i < nums.size() && cur == nums[i]; cnt++, i++);

            if (cur != pre + 1) {
                if (p1 != 0 || p2 != 0) return false;
                c1 = cnt; c2 = 0; c3 = 0;

            } else {
                if (cnt < p1 + p2) return false;
                c1 = std::max(0, cnt - (p1 + p2 + p3));
                c2 = p1;
                c3 = p2 + std::min(p3, cnt - (p1 + p2));
            }
        }

        return (p1 == 0 && p2 == 0);
    }
};
```
```c++
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> fre;
        unordered_map<int, int> need;
        for (auto n: nums) {
            fre[n]++;
        }
        for (int n: nums) {
            if (fre[n] == 0) {
                continue;
            } else if (need[n] > 0) {
                fre[n]--;
                need[n]--;
                need[n + 1]++;
            } else if (fre[n + 1] > 0 && fre[n + 2] > 0) {
                fre[n]--;
                fre[n + 1]--;
                fre[n + 2]--;
                need[n + 3]++;
            } else {
                return false;
            }
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> fre;
        unordered_map<int, int> need;
        for (auto n: nums) {
            fre[n]++;
        }
        for (int n: nums) {
            if (fre[n] == 0) {
                continue;
            } else if (need[n] > 0) {
                need[n]--;
                need[n + 1]++;
            } else if (fre[n + 1] > 0 && fre[n + 2] > 0) {
                fre[n + 1]--;
                fre[n + 2]--;
                need[n + 3]++;
            } else {
                return false;
            }
            fre[n]--;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        unordered_map<int, int> fre;
        unordered_map<int, int> need;
        for (auto n: nums) {
            fre[n]++;
        }
        for (int n: nums) {
            if (fre[n] == 0) {
                continue;
            } else if (need[n] > 0) {
                need[n]--;
                need[n + 1]++;
            } else if (fre[n + 1] > 0 && fre[n + 2] > 0) {
                fre[n + 1]--;
                fre[n + 2]--;
                need[n + 3]++;
            } else {
                return false;
            }
            fre[n]--;
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    bool isPossible(vector<int>& nums) {
        int len = nums.size();
        if (len == 0) return false;
        int start = nums[0];
        int end = nums[len - 1];
        vector<int> counts(end - start + 1);
        for(int num : nums) {
            counts[num - start]++;
        }
        int i = 0;
        while (i < counts.size()) {
            if (counts[i] == 0) {
                i++;
                continue;
            }
            counts[i]--;
            int j = i + 1;
            while(j < counts.size()) {
                if (counts[j] > counts[j - 1]) {
                    counts[j]--;
                    j++;
                } else {
                    break;
                }
            }
                        
            if (j - i <= 2) {
                return false;
            }
        }
        return true;
    }
};
```