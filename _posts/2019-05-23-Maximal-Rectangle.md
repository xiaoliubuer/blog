---
layout: post
title:  "85. Maximal Rectangle"
date: 2019-05-23 19:52:00 -0400
categories: articles
---

Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.

Example:
```
Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6
```

# Similiar
[84. Largest Rectangle in Histogram](http://localhost:4000/articles/2019/04/11/Largest-Rectangle-in-Histogram.html)

```c++
class Solution {
public:
int maximalRectangle(vector<vector<char> > &matrix) {
    if(matrix.empty()){
        return 0;
    }
    int maxRec = 0;
    vector<int> height(matrix[0].size(), 0);

    for(int i = 0; i < matrix.size(); i++){
        for(int j = 0; j < matrix[0].size(); j++){
            if(matrix[i][j] == '0'){
                height[j] = 0;
            }
            else{
                height[j]++;
            }
        }
        //4 0 0 3 0
        maxRec = max(maxRec, largestRectangleArea(height));
    }
    return maxRec;
}

int largestRectangleArea(vector<int> &height) {
    stack<int> s;
    height.push_back(0);
    int maxSize = 0;
    for(int i = 0; i < height.size(); i++){
        if(s.empty() || height[i] >= height[s.top()]){
            s.push(i);
        }
        else{
            int temp = height[s.top()];
            s.pop();
            maxSize = max(maxSize, temp * (s.empty() ? i : i - 1 - s.top()));
            i--;
        }
    }
    return maxSize;
}
};
```