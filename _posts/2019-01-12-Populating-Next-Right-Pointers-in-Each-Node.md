---
layout: post
title:  "116. Populating Next Right Pointers in Each Node"
date: 2019-01-12 19:54:23 -0400
categories: articles
---
Given a binary tree
```
struct TreeLinkNode {
  TreeLinkNode *left;
  TreeLinkNode *right;
  TreeLinkNode *next;
}
```
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.

Initially, all next pointers are set to NULL.

Note:
```
You may only use constant extra space.
Recursive approach is fine, implicit stack space does not count as extra space for this problem.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
```
Example:
```
Given the following perfect binary tree,

     1
   /  \
  2    3
 / \  / \
4  5  6  7
```
After calling your function, the tree should look like:
```
     1 -> NULL
   /  \
  2 -> 3 -> NULL
 / \  / \
4->5->6->7 -> NULL
```
# Function signature
```c++
class Solution {
public:
    void connect(TreeLinkNode *root) {
        
    }
};
```
# 题意
就是把一棵这样的树，左子树的next指向右子树，然后最右边的指向NULL。
# 想法
如果想往右边指，那就得知道右边是谁。如何知道右边是谁呢？？
那就BFS，用一个queue，放进去，拿出来，指向下一个跳出来的。但是如何处理这个
# 尝试解解
```c++
// 尝试了一次屎一样的做法
// 终于accepted！了！ 所以用queue的思路是对的，只是有很多的细节，corner case需要考虑。
class Solution {
public:
    void connect(TreeLinkNode *root) {
    	if (!root) return;
    	queue<TreeLinkNode*> myQueue;
    	myQueue.push(root);
    	myQueue.push(NULL);
    	TreeLinkNode* curr = root;
    	while(!myQueue.empty()){
    		TreeLinkNode* first = myQueue.front();
    		myQueue.pop();
    		if (first) first->next = myQueue.front();
    		if (first && first->left) myQueue.push(first->left);
    		if (first && first->right) myQueue.push(first->right);
    		if (myQueue.front() == NULL && myQueue.size() > 1)
    			myQueue.push(NULL);
    	}
    }
};
```
# 参考答案
```c++
// 这个答案牛！！！
class Solution {
public:
    void connect(TreeLinkNode *root) {
        if (!root) return;
        if (root->left) root->left->next = root->right;
        if (root->right && root->next) root->right->next = root->next->left; // 这一句非常巧妙
        connect(root->left);
        connect(root->right);
    }
};
```
```c++

```

```c++
《要不要去山顶看日出》

作者：林旷

我是一个要去山顶看日出的人，你要不要跟我一起？

我看惯了门前的日出，她很美，但我没有见过山顶的日出，
我想去，你要不要陪我一起？

我现在的车虽然不是名牌，但出来兜风很过瘾，你要不要来试一试。
我虽然暂时没有驾照，但我相信不会把你带到沟里，这个请你放心。

如果你一定需要我通过考试，拿个驾照再出发，那没问题，我只需要一点点时间而已。

但是，上路了，你就是我看地图的导航，我是你的专属司机，
我相信你会陪我到终点，也请你相信我不会把你带到沟里。

我希望这是专属于我们俩的旅行，你可以给大家分享路上的风景，
但是，碰上刮风下雨，希望你可以靠在我怀里。

我希望你能说服你的父母放心，我知道她们很疼爱你，
但是，这次是我们的旅行。

路上可能有美丽的风景，路上可能也会很艰辛，但只要有你，
我会陪你共同去面对。

我真的很想去山顶看日出，真的，你要不要一起同行？

// 刷着刷着莫名其妙和准老婆争吵了一小下，突然有感而发写了这首土情诗，啊哈哈哈！！
```