---
layout: post
title:  "558. Quad Tree Intersection"
date: 2019-08-11 16:38:00 -0400
categories: articles
---
A quadtree is a tree data in which each internal node has exactly four children: topLeft, topRight, bottomLeft and bottomRight. Quad trees are often used to partition a two-dimensional space by recursively subdividing it into four quadrants or regions.

We want to store True/False information in our quad tree. The quad tree is used to represent a N * N boolean grid. For each node, it will be subdivided into four children nodes until the values in the region it represents are all the same. Each node has another two boolean attributes : isLeaf and val. isLeaf is true if and only if the node is a leaf node. The val attribute for a leaf node contains the value of the region it represents.

For example, below are two quad trees A and B:

A:
```
+-------+-------+   T: true
|       |       |   F: false
|   T   |   T   |
|       |       |
+-------+-------+
|       |       |
|   F   |   F   |
|       |       |
+-------+-------+
topLeft: T
topRight: T
bottomLeft: F
bottomRight: F
```
B: 
```              
+-------+---+---+
|       | F | F |
|   T   +---+---+
|       | T | T |
+-------+---+---+
|       |       |
|   T   |   F   |
|       |       |
+-------+-------+
topLeft: T
topRight:
     topLeft: F
     topRight: F
     bottomLeft: T
     bottomRight: T
bottomLeft: T
bottomRight: F
```

Your task is to implement a function that will take two quadtrees and return a quadtree that represents the logical OR (or union) of the two trees.
```
A:                 B:                 C (A or B):
+-------+-------+  +-------+---+---+  +-------+-------+
|       |       |  |       | F | F |  |       |       |
|   T   |   T   |  |   T   +---+---+  |   T   |   T   |
|       |       |  |       | T | T |  |       |       |
+-------+-------+  +-------+---+---+  +-------+-------+
|       |       |  |       |       |  |       |       |
|   F   |   F   |  |   T   |   F   |  |   T   |   F   |
|       |       |  |       |       |  |       |       |
+-------+-------+  +-------+-------+  +-------+-------+
```
Note:
```
Both A and B represent grids of size N * N.
N is guaranteed to be a power of 2.
If you want to know more about the quad tree, you can refer to its wiki.
The logic OR operation is defined as this: "A or B" is true if A is true, or if B is true, or if both A and B are true.
```
```c++
class Solution {
public:
    Node* intersect(Node* quadTree1, Node* quadTree2) {
        std::ios::sync_with_stdio(false);
        if (quadTree1->isLeaf)
            return quadTree1->val ? quadTree1 : quadTree2;
        if (quadTree2->isLeaf)
            return quadTree2->val ? quadTree2 : quadTree1;
        
        Node* tl = intersect(quadTree1->topLeft, quadTree2->topLeft);
        Node* tr = intersect(quadTree1->topRight, quadTree2->topRight);
        Node* bl = intersect(quadTree1->bottomLeft, quadTree2->bottomLeft);
        Node* br = intersect(quadTree1->bottomRight, quadTree2->bottomRight);
        
        if (tl->isLeaf && tr->isLeaf && bl->isLeaf && br->isLeaf && tl->val && tr->val && bl->val && br->val) 
            return new Node(true, true, nullptr, nullptr, nullptr, nullptr);
        else 
            return new Node(false, false, tl, tr, bl, br);
    }
};
```
```c++
class Solution {
public:
    Node* intersect(Node* quadTree1, Node* quadTree2) {
        if (quadTree1->isLeaf) return quadTree1->val ? quadTree1 : quadTree2;
        
        if (quadTree2->isLeaf) return quadTree2->val ? quadTree2 : quadTree1;
        
        Node *tl = intersect(quadTree1->topLeft, quadTree2->topLeft);
        Node *tr = intersect(quadTree1->topRight, quadTree2->topRight);
        Node *bl = intersect(quadTree1->bottomLeft, quadTree2->bottomLeft);
        Node *br = intersect(quadTree1->bottomRight, quadTree2->bottomRight);
        
        if (tl->val == tr->val && tl->val == bl->val && tl->val == br->val && 
            tl->isLeaf && tr->isLeaf && bl->isLeaf && br->isLeaf) 
        {
              return new Node(tl->val, true, NULL, NULL, NULL, NULL);
        } else 
        {
              return new Node(false, false, tl, tr, bl, br);
        }        
    }
};
```
```c++
class Solution {
public:
    Node* intersect(Node* q1, Node* q2) {
        Node* res = new Node();
        res->isLeaf = false;
        res->val = NULL;
        
        if (q1->isLeaf && q2->isLeaf) {
            res->isLeaf = true;
            res->val = q1->val || q2->val;
            res->topLeft = NULL;
            res->topRight = NULL;
            res->bottomLeft = NULL;
            res->bottomRight = NULL;
            return res;	
        } 
		else if (q1->isLeaf)
		{
			// Return true immediately if one leaf node has true value 
            if (q1->val)    return new Node(true, true, NULL, NULL, NULL, NULL);
			
            res->topLeft = intersect(q1, q2->topLeft);
            res->topRight = intersect(q1, q2->topRight);
            res->bottomLeft = intersect(q1, q2->bottomLeft);
            res->bottomRight = intersect(q1, q2->bottomRight);	
        } 
		else if (q2->isLeaf) 
		{
			// Return true immediately if one leaf node has true value 
            if (q2->val)    return new Node(true, true, NULL, NULL, NULL, NULL);
			
            res->topLeft = intersect(q1->topLeft, q2);
            res->topRight = intersect(q1->topRight, q2);
            res->bottomLeft = intersect(q1->bottomLeft, q2);
            res->bottomRight = intersect(q1->bottomRight, q2);
        } 
		else 
		{
            res->topLeft = intersect(q1->topLeft, q2->topLeft);
            res->topRight = intersect(q1->topRight, q2->topRight);
            res->bottomLeft = intersect(q1->bottomLeft, q2->bottomLeft);
            res->bottomRight = intersect(q1->bottomRight, q2->bottomRight);
        }
        
        // If all four children have the same val and are leaves, combine them into one grid
        if (res->topLeft->isLeaf && res->topRight->isLeaf && res->bottomLeft->isLeaf && res->bottomRight->isLeaf && res->topLeft->val == res->topRight->val && res->topRight->val == res->bottomLeft->val && res->bottomLeft->val == res->bottomRight->val && res->bottomRight->val == res->topLeft->val)
            return new Node(res->topLeft, true, NULL, NULL, NULL, NULL);
        
        return res;
    }
};
```