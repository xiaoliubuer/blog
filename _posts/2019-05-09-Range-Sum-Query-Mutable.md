---
layout: post
title:  "307. Range Sum Query - Mutable"
date: 2019-05-09 17:43:00 -0400
categories: articles
---

Given an integer array nums, find the sum of the elements between indices i and j (i ≤ j), inclusive.

The update(i, val) function modifies nums by updating the element at index i to val.

Example:

Given nums = [1, 3, 5]

sumRange(0, 2) -> 9
update(1, 2)
sumRange(0, 2) -> 8
Note:

The array is only modifiable by the update function.
You may assume the number of calls to update and sumRange function is distributed evenly.

```java
int[] tree;
int n;
public NumArray(int[] nums) {
    if (nums.length > 0) {
        n = nums.length;
        tree = new int[n * 2];
        buildTree(nums);
    }
}
private void buildTree(int[] nums) {
    for (int i = n, j = 0;  i < 2 * n; i++,  j++)
        tree[i] = nums[j];
    for (int i = n - 1; i > 0; --i)
        tree[i] = tree[i * 2] + tree[i * 2 + 1];
}

void update(int pos, int val) {
    pos += n;
    tree[pos] = val;
    while (pos > 0) {
        int left = pos;
        int right = pos;
        if (pos % 2 == 0) {
            right = pos + 1;
        } else {
            left = pos - 1;
        }
        // parent is updated after child is updated
        tree[pos / 2] = tree[left] + tree[right];
        pos /= 2;
    }
}

public int sumRange(int l, int r) {
    // get leaf with value 'l'
    l += n;
    // get leaf with value 'r'
    r += n;
    int sum = 0;
    while (l <= r) {
        if ((l % 2) == 1) {
           sum += tree[l];
           l++;
        }
        if ((r % 2) == 0) {
           sum += tree[r];
           r--;
        }
        l /= 2;
        r /= 2;
    }
    return sum;
}

```

```c++
class NumArray {
public:

    class SegTreeNode
    {
    public:
        SegTreeNode(int left, int right){
            this->m_left = left;
            this->m_right= right;
            this->sum = 0;
            this->left = NULL;
            this->right= NULL;
        };

    int m_left;
    int m_right;
    int sum;
    SegTreeNode* left;
    SegTreeNode* right;
    };


    SegTreeNode* root;

    SegTreeNode* buildTree(vector<int> nums, int left, int right){
        if ( left > right){
            return NULL;
        }

        SegTreeNode* cur = new SegTreeNode(left, right);
        if ( left == right ) 
            cur.sum = nums[left];
        else{
            int mid = left + (right - left)/2;
            cur->left = buildTree(nums, left, mid);
            cur->right= buildTree(nums, mid+1,right);
            cur->sum = cur->left->sum + cur->right->sum;
        }
        return cur;
    }

    NumArray(vector<int> nums) {
        root = buildTree(nums, 0, nums.size() - 1);
    }



    void updateHelper(SegTreeNode* root, int i, int val){
        if (root->m_left == root->m_right){
            root->sum = val;
            return;
        }
        int mid = root->m_left + (root->m_right - root->m_left)/2;

        if ( i <= mid){
            updateHelper(root->left, i, val);
        }
        else{
            updateHelper(root->right, i, val);
        }
        root->sum = root->left->sum + root->right->sum;
}

    void update(int i, int val) {
        updateHelper(root, i, val);
    }
    
    int sumRangeHelper(SegTreeNode* root, int left, int right){
        if ( root->m_right == right && root->m_left == left) return root->sum;

        int mid = root->m_left + (root->m_right - root->m_left)/2;

        if ( right <= mid ){
            return sumRangeHelper(root->left, left, right);
        }
        else if ( left >= mid + 1){
            return sumRangeHelper(root->right, left, right);
        }
        else{
            return sumRangeHelper(root->left, left, mid) + sumRangeHelper(root->right, mid+1, right);
        }
    }

    int sumRange(int i, int j) {
        return sumRangeHelper(root, i, j);
    }
};
```