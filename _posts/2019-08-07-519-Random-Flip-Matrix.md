---
layout: post
title:  "519. Random Flip Matrix"
date: 2019-08-07 20:19:00 -0400
categories: articles
---
You are given the number of rows n_rows and number of columns n_cols of a 2D binary matrix where all values are initially 0. Write a function flip which chooses a 0 value uniformly at random, changes it to 1, and then returns the position [row.id, col.id] of that value. Also, write a function reset which sets all values back to 0. Try to minimize the number of calls to system's Math.random() and optimize the time and space complexity.

Note:
````
1 <= n_rows, n_cols <= 10000
0 <= row.id < n_rows and 0 <= col.id < n_cols
flip will not be called when the matrix has no 0 values left.
the total number of calls to flip and reset will not exceed 1000.
```
Example 1:
```
Input: 
["Solution","flip","flip","flip","flip"]
[[2,3],[],[],[],[]]
Output: [null,[0,1],[1,2],[1,0],[1,1]]
```
Example 2:
```
Input: 
["Solution","flip","flip","reset","flip"]
[[1,2],[],[],[],[]]
Output: [null,[0,0],[0,1],null,[0,0]]
```
Explanation of Input Syntax:
```
The input is two lists: the subroutines called and their arguments. Solution's constructor has two arguments, n_rows and n_cols. flip and reset have no arguments. Arguments are always wrapped with a list, even if there aren't any.
```

```c++
class Solution {
public:
    int m,n;
    unordered_set<int> flag;
    Solution(int n_rows, int n_cols):m(n_rows),n(n_cols),flag() {       
    }
    
    vector<int> flip() {
        int index = rand()%(m*n);
        while (flag.find(index) !=  flag.end())
            index = rand()%(m*n);
        flag.insert(index);
        return vector<int>{index/n,index%n};
    }
    
    void reset() {
        flag.clear();
    }
};
```

```c++
class Solution {
public:
    Solution(int n_rows, int n_cols) {
        row = n_rows; col = n_cols;
        size = row * col;
    }
    
    vector<int> flip() {
        int id = rand() % size, val = id;
        --size;
        if (m.count(id)) id = m[id]; //if id already visited, find m[id]; otherwise return id
        m[val] = m.count(size) ? m[size] : size; // exchange current id/val with the tail or m[tail] if tail has alreay been visited
        return {id / col, id % col};
    }
    
    void reset() {
        m.clear();
        size = row * col;
    }

private:
    int row, col, size;
    unordered_map<int, int> m;
};
```

```c++
class Solution {
public:
    Solution(int n_rows, int n_cols) {
        row = n_rows; col = n_cols;
    }
    
    vector<int> flip() {
        while (true) {
            int val = rand() % (row * col);
            if (!flipped.count(val)) {
                flipped.insert(val);
                return {val / col, val % col};
            }
        }
    }
    
    void reset() {
        flipped.clear();
    }

private:
    int row, col;
    unordered_set<int> flipped;
};
```
```c++
class Solution {
public:
    Solution(int n_rows, int n_cols):nRows(n_rows), nCols(n_cols) {
    }
    
    vector<int> flip() {
        int iZero = rand() % (nRows*nCols);
        if(onePos.find(iZero) == onePos.end())
        {
            onePos.insert(iZero);
            return {iZero % nRows, iZero / nRows};
        }
        else
        for(int i = 0 ;i < nRows*nCols ;i++)
        {
            iZero++;
            if(iZero == nRows*nCols)
                iZero = 0;
            
            if(onePos.find(iZero) == onePos.end())
            {
                onePos.insert(iZero);
                return {iZero % nRows, iZero / nRows};
            }
        }
        
        return {};
    }
    
    void reset() {
        onePos.clear();
    }
    
private:
    unordered_set<int> onePos;
    int nRows,nCols;
};
```