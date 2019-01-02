---
layout: post
title:  "486. Predict the Winner"
date: 2018-12-29 09:47:23 -0400
categories: articles
---

Given an array of scores that are non-negative integers. Player 1 picks one of the numbers from either end of the array followed by the player 2 and then player 1 and so on. Each time a player picks a number, that number will not be available for the next player. This continues until all the scores have been chosen. The player with the maximum score wins.

Given an array of scores, predict whether player 1 is the winner. You can assume each player plays to maximize his score.

Example 1:
```
Input: [1, 5, 2]
Output: False
Explanation: Initially, player 1 can choose between 1 and 2. 
If he chooses 2 (or 1), then player 2 can choose from 1 (or 2) and 5. If player 2 chooses 5, then player 1 will be left with 1 (or 2). 
So, final score of player 1 is 1 + 2 = 3, and player 2 is 5. 
Hence, player 1 will never be the winner and you need to return False.
```
Example 2:
```
Input: [1, 5, 233, 7]
Output: True
Explanation: Player 1 first chooses 1. Then player 2 have to choose between 5 and 7. No matter which number player 2 choose, player 1 can choose 233.
Finally, player 1 has more score (234) than player 2 (12), so you need to return True representing player1 can win.
```
Note:

1 <= length of the array <= 20.
Any scores in the given array are non-negative integers and will not exceed 10,000,000.
If the scores of both players are equal, then player 1 is still the winner.

# 题意
给了一组非负数，玩家1从头或尾选一个数，然后玩家2从头或尾选一个，以此类推，最后挑选的数的总和最大的玩家获胜。所以你就写算法来判定玩家1会不会获胜。

所以计算的方式就是，如果玩家1获胜，那么就要确保无论玩家1选了left还是right，那么玩家2的选项一定要确保有一个是fail的。

1. Helper function
```c++
// OneWin
bool OneWin(vector<int> nums, int l, int r, int sum1, int sum2){
	if ( l > r )
		return sum1 >= sum2;
	else
		return !TwoWin(nums, l + 1, r, sum1 + nums[l], sum2) || !TwoWin(nums, l, r - 1, sum1 + nums[r], sum2)
}
// TwoWin
bool TwoWin(vector<int> nums, int l, int r, int sum1, int sum2){
	if ( l > r )
		return sum1 < sum2;
	else
		return !OneWin(nums, l + 1, r, sum1, sum2 + nums[l]) || !OneWin(nums, l, r - 1, sum1, sum2 + nums[r])
}
```
2. Main function
```c++
class Solution {
public:
    bool PredictTheWinner(vector<int>& nums) {
    	if (nums.size() == 0) return false;
        return OneWin(nums, 0, nums.size() - 1, 0, 0);
    }
};
```
# 参考答案
```c++
class Solution {
public:
    // OneWin
bool OneWin(vector<int> nums, int l, int r, int sum1, int sum2){
	if ( l > r )
		return sum1 >= sum2;
	else
		return !TwoWin(nums, l + 1, r, sum1 + nums[l], sum2) || !TwoWin(nums, l, r - 1, sum1 + nums[r], sum2);
}
// TwoWin
bool TwoWin(vector<int> nums, int l, int r, int sum1, int sum2){
	if ( l > r )
		return sum1 < sum2;
	else
		return !OneWin(nums, l + 1, r, sum1, sum2 + nums[l]) || !OneWin(nums, l, r - 1, sum1, sum2 + nums[r]);
}
bool PredictTheWinner(vector<int>& nums) {
    	if (nums.size() == 0) return false;
        return OneWin(nums, 0, nums.size() - 1, 0, 0);
    }
};
```
