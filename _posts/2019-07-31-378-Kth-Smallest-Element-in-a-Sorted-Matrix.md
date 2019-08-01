---
layout: post
title:  "378. Kth Smallest Element in a Sorted Matrix"
date: 2019-07-31 20:47:00 -0400
categories: articles
---
Given a n x n matrix where each of the rows and columns are sorted in ascending order, find the kth smallest element in the matrix.

Note that it is the kth smallest element in the sorted order, not the kth distinct element.

Example:
```
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,

return 13.
```
Note: 
You may assume k is always valid, 1 ≤ k ≤ n2.

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int m = matrix.size(), n = matrix[0].size();
        priority_queue<int, vector<int>> pq;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                pq.emplace(matrix[i][j]);
                if (pq.size() > k) {
                    pq.pop();
                }
            }
        }
        int result = pq.top();
        return result;
    }
};
```

```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        ios_base::sync_with_stdio(0);
        cin.tie(0);
        vector<int>a;
        for(int i=0;i<matrix.size();i++)
        {
            for(int j=0;j<matrix[0].size();j++)
            {
                a.push_back(matrix[i][j]);
            }
        }
        sort(a.begin(),a.end());
        return a[k-1];
    }
};
```
```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        int l=matrix[0][0],r=matrix[n-1][n-1];
        while(l<r){
            int m=l+(r-l)/2;
            int total=0; //# of elements < K
            for(int i=0;i<n;i++){
                int pos = upper_bound(matrix[i].begin(), matrix[i].end(), m) - matrix[i].begin();
                total += pos;       
        }
        if(total>=k)
            r=m;
        else
            l=m+1;
            
        }
        return r;
    }
    
};
```
```c++
class Solution
{
public:
	int kthSmallest(vector<vector<int>>& matrix, int k)
	{
		int n = matrix.size();
		int le = matrix[0][0], ri = matrix[n - 1][n - 1];
		int mid = 0;
		while (le < ri)
		{
			mid = le + (ri-le)/2;
			int num = 0;
			for (int i = 0; i < n; i++)
			{
				int pos = upper_bound(matrix[i].begin(), matrix[i].end(), mid) - matrix[i].begin();
				num += pos;
			}
			if (num < k)
			{
				le = mid + 1;
			}
			else
			{
				ri = mid;
			}
		}
		return le;
	}
};
```