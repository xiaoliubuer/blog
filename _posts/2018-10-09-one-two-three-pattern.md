---
layout: post
title:  "132. Pattern"
date: 2018-10-09 18:49:23 -0400
categories: articles
---

Given a sequence of n integers a1, a2, ..., an, a 132 pattern is a subsequence ai, aj, ak such that i < j < k and ai < ak < aj. Design an algorithm that takes a list of n numbers as input and checks whether there is a 132 pattern in the list.

Note: n will be less than 15,000.

Example 1:
Input: [1, 2, 3, 4]

Output: False

Explanation: There is no 132 pattern in the sequence.
Example 2:
Input: [3, 1, 4, 2]

Output: True

Explanation: There is a 132 pattern in the sequence: [1, 4, 2].
Example 3:
Input: [-1, 3, 2, 0]

Output: True

Explanation: There are three 132 patterns in the sequenc

思路:

就是判断这个数组里面有没有存在 1 3 2 这样大小顺序的3个元素。如果存在就是true，否则就是false。
最直接的想法就是想用一个stack，放一个，如果在size等于1的时候，有比他大的，就放第二个，然后在size等于2的时候，
如果有在他们之间的数存在，就返回true。
这样的时间复杂度是N的三次方

所以这个是很烂的的方法。

能不能更快？？

如果我一遍扫，用两个变量，表示当前已经出现了，可能回符合条件的最小，最大。
然后如果有比最大的更大，那就最大变更大，如果出现有比最小更小，先保存下来，直到出现了比现在最大更大或相等的，才把最小的
赋值成当那个。一直保持长着的几张嘴，如果有新符合条件的就加入进来或者是更新现在的嘴的大小。然后发现有符合条件的，就是一口吃了。
```c++
//Answer:
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        if (nums.size() < 3) return false;
        stack<int> mystack;
        vector<int> min_nums(nums.size());
        min_nums[0] = nums[0];

        for (int i = 1; i < nums.size(); ++i){
        	min_nums[i] = min(min_nums[i-1], nums[i]);
        }

        for (int i = nums.size()-1; i >= 0 ; i--){
        	if (nums[i] > min_nums[i]){
        		while(!mystack.empty() && mystack.top() <= min_nums[i])
        			mystack.pop();
        		if (!mystack.empty() && mystack.top() < nums[i])
        			return true;
        		mystack.push(nums[i]);
        	}
        }
        return false;
    }
};
```
这个答案相当诡异，怎么能想出这样的答案呢？？非常有意思。用一个数组记录从前往后当前最小的值(1)。

然后从后往前遍历，用一个stack来保存当前的最小中间值(2),然后比较看看存不存在一个(3)大于stack里面的这个最小。
如果大于就赶快返回true.

这道题太tm有意思啦。


