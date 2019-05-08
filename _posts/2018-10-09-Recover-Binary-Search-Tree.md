---
layout: post
title:  "99. Recover Binary Search Tree"
date: 2018-10-09 23:57:23 -0400
categories: articles
---
Two elements of a binary search tree (BST) are swapped by mistake.

Recover the tree without changing its structure.

Example 1:
```
Input: [1,3,null,null,2]

   1
  /
 3
  \
   2

Output: [3,1,null,null,2]

   3
  /
 1
  \
   2
Example 2:

Input: [3,1,4,null,null,2]

  3
 / \
1   4
   /
  2

Output: [2,1,4,null,null,3]

  2
 / \
1   4
   /
  3
```
Follow up:
3 2 1
2 1 3


A solution using O(n) space is pretty straight forward.
Could you devise a constant space solution?
# Function signature
```c++
class Solution {
public:
    void recoverTree(TreeNode* root) {
        
    }
};
```
# 题意:
就是一棵BST上有两个节点的位置被调换了。找出来并且恢复这个原来的二叉树。
# 思路:
有意思，因为是BST，所以要满足所有的BST的特性，左子树永远小于等于root，右子树永远大于root

他说是有两个node给放错了。擦！！是谁放错的！！站起来！！

哎，我擦

所以就是要找到哪两个不对劲的狗男女，然后把他们交换回来。

那么问题就来了，怎么找？

所以大致的想法就是我们就遍历这棵树，如果发现异样。就保存下来。
然后继续，所以如果有问题，会有怎样的现象？？

__重点： 一定会出现一个node的 Predecessor 前驱节点大于它, 或者一定存在一个节点的 Successor 后继节点 小于它。__

其实发现的情况就是解决:

1 2 3 4 5 

case1: 1 3 2 4 5
case2: 1 7 3 4 5 6 2 8 

相邻的两个换了，还是间隔的两个换了的问题。
所以我们要做的就是，找到一个节点，他的predecessor大于它。
在找到一个节点，他的successor小于它。

所以当然可以找，一个节点，他的successor小于它，另一个节点，还是他的successor小于它。
总之就是以这样的方式，找到两个节点，然后交换他们，然后就做出来啦。

__注意，BST的问题一定要转化成升序数组来观察！__

所以怎么来

哎我擦，答案真香！！！基本有这个大致思路，到细节就开始想不到了～～

所以是怎样？就是一定还是要有两个指针，指向这两个出了问题的点
```c++
// Accepted! 2019-05-08
class Solution {
public:
    TreeNode *first = nullptr, *second = nullptr;
    TreeNode* pre = nullptr;
    
    void helper(TreeNode* root ){
        if ( !root ) return;
        helper(root->left);
        // You can't get your precessor and successor at current node in BST
        if ( pre && pre->val > root->val ) {
            if ( !first && !second ){
                first = pre;
                second = root;
            }
            else{
                second = root;
            }
        }
        pre = root;
        helper(root->right);
    }
    void recoverTree(TreeNode* root) {
        helper(root);
        swap(first->val, second->val);
    }
};
```
```c++
//Answer
class Solution {
    TreeNode* first=NULL;
    TreeNode* second=NULL;
    TreeNode* prev = new TreeNode(INT_MIN);
public:
    void recoverTree(TreeNode* root) {
        help(root);
        swp(first->val, second->val);
    }
    
    void help(TreeNode* root){
        if(root==NULL)  return;
        help(root->left);
        if(first==NULL && prev->val >= root->val)   first=prev; // 这里非常关键
        if(first!=NULL && prev->val >= root->val)   second=root; // 为什么一个是 prev， 一个是root呢？
        prev=root;
        help(root->right);
    }
};
```

```c++
// Accepted !!!!
class Solution {
public:
    TreeNode* firstNode  = NULL;
    TreeNode* secondNode = NULL;
    TreeNode* preNode = NULL;
    void helper(TreeNode* root){
        if (!root) return;
        helper(root->left);
        if (preNode && preNode->val >= root->val){
            if ( firstNode == NULL ){
                firstNode = preNode;
                secondNode = root;
            }
            else
                secondNode = root;
        }        
        preNode = root; // 这里一定要放在去右边节点之前
        helper(root->right);
    }
    void recoverTree(TreeNode* root) {
        if (!root) return;
        helper(root);
        swap(firstNode->val, secondNode->val);
    }
};
```
```c++
//2019-01-11 Accepted！！
class Solution {
public:
    TreeNode* s1 = nullptr, *s2 = nullptr;
    TreeNode* temp;
    void helper(TreeNode* root){
        if (!root) return;
        helper( root->left );
        if (temp && temp->val > root->val){
            if ( s1 == nullptr){
                s1 = temp;
                s2 = root;
            }
            else
                s2 = root;
        }
        temp = root; //这里很重要！！！
        helper(root->right);
    }
    void recoverTree(TreeNode* root) {
        helper(root);
        swap(s1->val, s2->val);
    }
};
```

3  2  1

1 2 3 4 5 6 7 -> 1 2 7 4 5 6 3

7 4 和 6 3

7 3

我操，就是中序遍历，发现我操，异常顺序，就把这两个傻逼记下来。因为有可能不知这个傻逼导致的，可能是其中一个和另外的一个傻逼导致的。所以就继续，如果有发现异常。 就把把第二个指针进行一个update，最后把两个指针更新一下。
所以要中序遍历，中序遍历的时候就是按照大小顺序走的，而且是从第一个点开始的。所以我们需要把前一个点保存下来，让后一个点可以使用。

一道非常经典的题。帮助更深入的理解BST
