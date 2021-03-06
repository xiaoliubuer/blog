---
layout: post
title:  "1029. Two City Scheduling"
date: 2019-05-12 12:13:00 -0400
categories: articles
---
There are 2N people a company is planning to interview. The cost of flying the i-th person to city A is costs[i][0], and the cost of flying the i-th person to city B is costs[i][1].

Return the minimum cost to fly every person to a city such that exactly N people arrive in each city.

 

Example 1:
```
Input: [[10,20],[30,200],[400,50],[30,20]]
Output: 110
Explanation: 
The first person goes to city A for a cost of 10.
The second person goes to city A for a cost of 30.
The third person goes to city B for a cost of 50.
The fourth person goes to city B for a cost of 20.

The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.
```

Note:

1 <= costs.length <= 100
It is guaranteed that costs.length is even.
1 <= costs[i][0], costs[i][1] <= 1000


```c++
class Solution {
public:
    int twoCitySchedCost(vector<vector<int>>& costs) {
        sort(costs.begin(), costs.end(), [](const vector<int>& A, const vector<int>& B){
            return (A[0] - A[1] < B[0] - B[1]);
        });
        int total = 0;
        int n = costs.size() / 2;
        
        for ( int i = 0; i < n; i++)
            total += costs[i][0] + costs[i+n][1];
        return total;
    }
};
```