---
layout: post
title:  "302. Smallest Rectangle Enclosing Black Pixels"
date: 2019-07-29 19:50:00 -0400
categories: articles
---
An image is represented by a binary matrix with 0 as a white pixel and 1 as a black pixel. The black pixels are connected, i.e., there is only one black region. Pixels are connected horizontally and vertically. Given the location (x, y) of one of the black pixels, return the area of the smallest (axis-aligned) rectangle that encloses all black pixels.

Example:
```
Input:
[
  "0010",
  "0110",
  "0100"
]
and x = 0, y = 2
```
Output: 6

```c++
// Binary Search
class Solution {
public:
    int minArea(vector<vector<char>>& image, int x, int y) {
        int m = image.size(), n = image[0].size();
        int up = binary_search(image, true, 0, x, 0, n, true);
        int down = binary_search(image, true, x + 1, m, 0, n, false);
        int left = binary_search(image, false, 0, y, up, down, true);
        int right = binary_search(image, false, y + 1, n, up, down, false);
        return (right - left) * (down - up);
    }
    int binary_search(vector<vector<char>> &image, bool h, int i, int j, int low, int high, bool opt) {
        while (i < j) {
            int k = low, mid = (i + j) / 2;
            while (k < high && (h ? image[mid][k] : image[k][mid]) == '0') ++k;
            if (k < high == opt) j = mid;
            else i = mid + 1;
        }
        return i;
    }
};
```
```c++
class Solution {
public:
    int minArea(vector<vector<char>>& image, int x, int y) {
        if(image.size() == 0)
            return 0;
        int n = image.size(), m = image[0].size();
        int miny = m - 1, maxy = 0, maxx = 0, minx = n;
        dfs(image, minx, maxx, miny, maxy, x, y);
        return (maxx - minx + 1)*(maxy - miny + 1);
    }
private:
    void dfs(vector<vector<char>>& image, int &minx, int &maxx, int &miny, int &maxy, int x, int y){
        minx = min(minx, x);
        maxx = max(maxx, x);
        miny = min(miny, y);
        maxy = max(maxy, y);
        image[x][y] = '0';
        if(x >= 1 && image[x - 1][y] == '1')
            dfs(image, minx, maxx, miny, maxy, x - 1, y);
        if(x < image.size() - 1 && image[x + 1][y] == '1')
            dfs(image, minx, maxx, miny, maxy, x + 1, y);
        if(y >= 1 && image[x][y - 1] == '1')
            dfs(image, minx, maxx, miny, maxy, x, y - 1);
        if(y < image[0].size() - 1 && image[x][y + 1] == '1')
            dfs(image, minx, maxx, miny, maxy, x, y + 1);
    }
};
```