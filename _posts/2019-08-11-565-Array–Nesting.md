---
layout: post
title:  "565. Array Nesting"
date: 2019-08-11 17:09:00 -0400
categories: articles
---
A zero-indexed array A of length N contains all integers from 0 to N-1. Find and return the longest length of set S, where S[i] = {A[i], A[A[i]], A[A[A[i]]], ... } subjected to the rule below.

Suppose the first element in S starts with the selection of element A[i] of index = i, the next element in S should be A[A[i]], and then A[A[A[i]]]… By that analogy, we stop adding right before a duplicate element occurs in S.

Example 1:
```
Input: A = [5,4,0,3,1,6,2]
Output: 4
```
Explanation: 
```
A[0] = 5, A[1] = 4, A[2] = 0, A[3] = 3, A[4] = 1, A[5] = 6, A[6] = 2.

One of the longest S[K]:
S[0] = {A[0], A[5], A[6], A[2]} = {5, 6, 2, 0}
```
Note:
```
N is an integer within the range [1, 20,000].
The elements of A are all distinct.
Each element of A is an integer within the range [0, N-1].
```
```c++
class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        int mx_len = 0;
        for(int i = 0; i < nums.size(); i++) {
            int j = i;
            int curlen = 0;
            while(nums[j] != -1) {
                int k = j;
                j = nums[j];
                nums[k] = -1;
                curlen++;
            }
            mx_len = max(mx_len, curlen);
        }
        return mx_len;
    }
};
```
```c++
class Solution {
public:
    int dfs(int cur, vector<int>& nums, vector<int>& seen) {
        if (seen[cur] > 0) return seen[cur];
        else if (seen[cur] == -1) return 0;
        seen[cur] = -1;
        int path = 1 + dfs(nums[cur], nums, seen);
        seen[cur] = path;
        return seen[cur];
    }
    
    int arrayNesting(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        else if (nums.size() == 1) return 1;
        vector<int> seen(nums.size());
        int ans = 1;
        for (int i = 0; i < nums.size(); i++) {
            if (seen[i] > 0) continue;
            int path = dfs(i, nums, seen);
            if (path > ans) ans = path;
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    
    int arrayNesting(vector<int>& nums) {
         int n = nums.size();
         int maxlen = 0;
         vector<int> visit(n);
         for(int i=0;i<n;i++){
            if(visit[i] > 0) continue;
            int len = 0;
            int m = i;
            do {
                len++;
                visit[m] = 1;
                m = nums[m];
            }while(m!=i);
            // int len = helper(nums,visit,i,i);
            maxlen = len > maxlen? len:maxlen;            
         }
        return maxlen;
    }
}
```c++
class Solution {
public:
	int arrayNesting(vector<int>& nums) {
		int res=0;
		for(int i=0;i<nums.size();i++){
			int len=0;
			for(int tem=i;nums[tem]>=0;){
				len++;
				int tem2;
				tem2=nums[tem];
				nums[tem]=-1;
				tem=tem2;
			}
			res=max(res,len);
			}
			return res;
		}


	};
```