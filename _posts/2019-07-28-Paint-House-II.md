---
layout: post
title:  "265. Paint House II"
date: 2019-07-28 19:28:00 -0400
categories: articles
---
There are a row of n houses, each house can be painted with one of the k colors. The cost of painting each house with a certain color is different. You have to paint all the houses such that no two adjacent houses have the same color.

The cost of painting each house with a certain color is represented by a n x k cost matrix. For example, costs[0][0] is the cost of painting house 0 with color 0; costs[1][2] is the cost of painting house 1 with color 2, and so on... Find the minimum cost to paint all houses.

Note:
All costs are positive integers.

Example:
```
Input: [[1,5,3],[2,9,4]]
Output: 5
```
Explanation: Paint house 0 into color 0, paint house 1 into color 2. Minimum cost: 1 + 4 = 5; 
             Or paint house 0 into color 2, paint house 1 into color 0. Minimum cost: 3 + 2 = 5. 
Follow up:
Could you solve it in O(nk) runtime?

```c++
class Solution {
public:
    int minCost(vector<vector<int>>& costs, int row, int idx){
        int costMin = INT_MAX;
        for(int i=0; i<costs[row].size(); i++){
            if(i != idx)
            {
                costMin = min(costMin, costs[row][i]);
            }
        }
        return costMin;
    }

    int minCostII(vector<vector<int>>& costs) {
        
        //special case
        if(costs.empty())
        {
            return  0;
        }
        
        if(costs.size() == 1)
        {
            return minCost(costs, 0, -1);
        }
        
        for(int i=1; i<costs.size(); i++)
        {
            for(int j=0; j<costs[0].size(); j++)
            {
                costs[i][j] += minCost(costs, i-1, j);
            }
        }
        return minCost(costs, costs.size()-1, -1);
    }
};
```