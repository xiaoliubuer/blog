---
layout: post
title:  "427. Construct Quad Tree"
date: 2019-08-02 19:44:00 -0400
categories: articles
---

We want to use quad trees to store an N x N boolean grid. Each cell in the grid can only be true or false. The root node represents the whole grid. For each node, it will be subdivided into four children nodes until the values in the region it represents are all the same.

Each node has another two boolean attributes : isLeaf and val. isLeaf is true if and only if the node is a leaf node. The val attribute for a leaf node contains the value of the region it represents.

Your task is to use a quad tree to represent a given grid. The following example may help you understand the problem better:

Given the 8 x 8 grid below, we want to construct the corresponding quad tree:
It can be divided according to the definition above:
The corresponding quad tree should be as following, where each node is represented as a (isLeaf, val) pair.

For the non-leaf nodes, val can be arbitrary, so it is represented as * .

Note:

N is less than 1000 and guaranteened to be a power of 2.
If you want to know more about the quad tree, you can refer to its wiki.

```c++
class Solution {
public:
    Node* construct(vector<vector<int>>& grid) {
        int size_ = grid.size();
        return construct_helper(grid, 0, 0, size_);
    }
    Node* construct_helper(vector<vector<int>>& grid, int x, int y, int step) {
        // Check whether it is a leaf node.
        int target_value = grid[x][y];
        if (isLeaf(grid, x, y, step)) {
            return new Node(target_value==1, true, NULL, NULL, NULL, NULL);
        }
        // Splits into four regions.
        step/=2;
        Node* top_left = construct_helper(grid, x, y, step);
        Node* top_right = construct_helper(grid, x, y+step, step);
        Node* bottom_left = construct_helper(grid, x+step, y, step);
        Node* bottom_right = construct_helper(grid, x+step, y+step, step);
        return new Node(true, false, top_left, top_right, bottom_left, bottom_right);
    }
    bool isLeaf(vector<vector<int>>& grid, int x, int y, int step) {
        int target_value = grid[x][y];
        for (int i = 0; i< step; ++i) {
            for (int j = 0; j< step; ++j) {
                if (grid[x+i][y+j] != target_value) {
                    return false;
                }
            }
        }
        return true;
    }
};
```
```c++
class Solution {
public:
    vector<vector<int>> grid_;
    vector<Node*> leafs;
    Node* construct(vector<vector<int>>& grid) {
        grid_ = move(grid);
        leafs.push_back(new Node(false, true, nullptr, nullptr, nullptr, nullptr));         leafs.push_back(new Node(true, true, nullptr, nullptr, nullptr, nullptr));           return construct(0, 0, grid_.size());
    }
    Node* construct(int r, int c, int size) {
        if (size == 1) return leafs[grid_[r][c]];
        size /= 2;
        Node* lu = construct(r, c, size);
        Node* ru = construct(r, c+size, size);
        Node* lb = construct(r+size, c, size);
        Node* rb = construct(r+size, c+size, size);
        if (lu == ru && lu == lb && lu == rb) {
            return lu;
        }
        return new Node(false, false, lu, ru, lb, rb);
    }
};
```
```c++
class Solution {
public:
  Node* construct(vector<vector<int>>& grid) {
    grid_ = move(grid);
    leafNodes_[0] = new Node(false, true, nullptr, nullptr, nullptr, nullptr);
    leafNodes_[1] = new Node(true,  true, nullptr, nullptr, nullptr, nullptr);
    return construct(0, 0, grid_.size());
  }
  
  Node* construct(int r, int c, int s) {
    if (s == 1)
      return leafNodes_[grid_[r][c]];
    s /= 2;
    Node* tl = construct(r,   c,   s);
    Node* tr = construct(r,   c+s, s);
    Node* bl = construct(r+s, c,   s);
    Node* br = construct(r+s, c+s, s);
    if (tl == tr && tl == bl && tl == br)
      return tl;
    return new Node(false, false, tl, tr, bl, br);
  }
  
private:
  vector<vector<int>> grid_;
  array<Node*, 2> leafNodes_;
};
```