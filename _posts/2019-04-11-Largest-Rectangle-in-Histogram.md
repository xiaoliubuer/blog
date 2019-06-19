---
layout: post
title:  "84. Largest Rectangle in Histogram"
date: 2019-04-11 08:43:00 -0400
categories: articles
---
Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].
The largest rectangle is shown in the shaded area, which has area = 10 unit.
Example:
```
Input: [2,1,5,6,2,3]
Output: 10
```

# Similiar
[85. Maximal Rectangle](http://localhost:4000/articles/2019/05/23/Maximal-Rectangle.html)

```c++
// Accepted !! 2019/05/24
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> buff;
        int n = heights.size();
        int res = 0;
        for ( int i = 0; i <= n; i++ ) {
            while( !buff.empty() && ( i == n || heights[i] <= heights[buff.top()])){ // Key part, why here???
                int top = buff.top();
                buff.pop();
                res = max(res, heights[top]*( buff.empty() ? i : i - buff.top() - 1));
            }
            buff.push(i);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> buff;
        int n = heights.size();
        int res = 0;
        for ( int i = 0; i <= n; i++ ) {
            while( !buff.empty() && ( i == n || heights[i] <= heights[buff.top()])){
                int top = buff.top();
                buff.pop();
                int width = 0;
                if ( buff.empty() )
                    width = i;
                else 
                    width = i - buff.top() - 1;
                res = max(res, heights[top]* width);
            }
            buff.push(i);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& A) {
        int n = A.size(), ans = 0, pos = 0;
        stack<int> st;
        for (int i = 0; i <= n; i++) {
            while (!st.empty() && (i == n || A[st.top()] >= A[i])) { // A[st.top()] >= A[i], current rectangle shorter than previous one
                pos = st.top(); 
                st.pop();
                ans = max(ans, A[pos] * (st.empty() ? i : i-st.top()-1));
            }
            st.push(i);
        }
        return ans;
    }
};
```
```java
public class Solution {
    public int calculateArea(int[] heights, int start, int end) {
        if (start > end)
            return 0;
        int minindex = start;
        for (int i = start; i <= end; i++)
            if (heights[minindex] > heights[i])
                minindex = i;
        return Math.max(heights[minindex] * (end - start + 1), Math.max(calculateArea(heights, start, minindex - 1), calculateArea(heights, minindex + 1, end)));
    }
    public int largestRectangleArea(int[] heights) {
        return calculateArea(heights, 0, heights.length - 1);
    }
}
```