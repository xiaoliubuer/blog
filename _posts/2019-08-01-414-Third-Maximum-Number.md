---
layout: post
title:  "414. Third Maximum Number"
date: 2019-08-01 20:52:00 -0400
categories: articles
---
Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).

Example 1:
```
Input: [3, 2, 1]

Output: 1

Explanation: The third maximum is 1.
```
Example 2:
```
Input: [1, 2]

Output: 2

Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
```
Example 3:
```
Input: [2, 2, 3, 1]

Output: 1

Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.
```

```c++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        set<int> s;
        for (int num: nums){
            s.insert(num);
            if (s.size() > 3) s.erase(s.begin());
        }
        return s.size() != 3? *s.rbegin(): *s.begin();
    }
};
```
```c++

class Solution {
public:
    int thirdMax(vector<int>& nums) {
        priority_queue<int> PQ;
        
        for(auto n: nums)
            PQ.push(n);
        int order = 1;
        int num = PQ.top();
        int M = num;
        while(order < 3 && !PQ.empty()) {
            if (num != PQ.top()) {
                order++;
                num = PQ.top();
            }
            PQ.pop();            
        }
        return order == 3 ? num : M;
    }
};
```

```c++
class Solution {
public:
    int thirdMax(vector<int>& nums) {
        priority_queue<int> PQ;
        
        for(auto n: nums)
            PQ.push(n);
        int order = 1;
        int num = PQ.top();
        int M = num;
        while(order < 3 && !PQ.empty()) {
            if (num != PQ.top()) {
                order++;
                num = PQ.top();
            }
            PQ.pop();            
        }
        return order == 3 ? num : M;
    }
};
```
```c++
class Solution {
public:
	int thirdMax(vector<int>& nums) {
		int len = nums.size();
		sort(nums.begin(), nums.end());
		int target = INT_MAX;
		int t = 0;
		int i = len;
		for (i; i > 0&&t<3; i--) {
			if (nums[i - 1] < target) {
				target = nums[i - 1];
				t++;
			}
		}
        if(t==2)return nums[len-1];
		return nums[i];
	}
};
```