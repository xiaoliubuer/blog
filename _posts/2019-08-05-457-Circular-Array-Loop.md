---
layout: post
title:  "457. Circular Array Loop"
date: 2019-08-05 19:18:00 -0400
categories: articles
---
You are given a circular array nums of positive and negative integers. If a number k at an index is positive, then move forward k steps. Conversely, if it's negative (-k), move backward k steps. Since the array is circular, you may assume that the last element's next element is the first element, and the first element's previous element is the last element.

Determine if there is a loop (or a cycle) in nums. A cycle must start and end at the same index and the cycle's length > 1. Furthermore, movements in a cycle must all follow a single direction. In other words, a cycle must not consist of both forward and backward movements.

Example 1:
```
Input: [2,-1,1,2,2]
Output: true
Explanation: There is a cycle, from index 0 -> 2 -> 3 -> 0. The cycle's length is 3.
```
Example 2:
```
Input: [-1,2]
Output: false
Explanation: The movement from index 1 -> 1 -> 1 ... is not a cycle, because the cycle's length is 1. By definition the cycle's length must be greater than 1.
```
Example 3:
```
Input: [-2,1,-1,-2,-2]
Output: false
Explanation: The movement from index 1 -> 2 -> 1 -> ... is not a cycle, because movement from index 1 -> 2 is a forward movement, but movement from index 2 -> 1 is a backward movement. All movements in a cycle must follow a single direction.
```
Note:
```
-1000 ≤ nums[i] ≤ 1000
nums[i] ≠ 0
1 ≤ nums.length ≤ 5000
```
Follow up:
Could you solve it in O(n) time complexity and O(1) extra space complexity?

```c++
class Solution {
public:
    bool circularArrayLoop(vector<int>& nums) {
        int MAXN = 2000;
        int n = nums.size();
        int round = 0;
        for(int i = 0; i < n; i ++){
            round ++;
            if(nums[i] > 1000) continue;
            int now = ((i+nums[i])%n+n)%n;
            if( now == i){
                continue;
            }
            cout << i << " " << now << " ";
            while(nums[now] <= 1000 && nums[i] * nums[now] > 0){
                int tmp = nums[now];
                while(tmp > 1000){
                    tmp -= 2000;
                }
                int pre = now;
                nums[now] += round * MAXN;
                now = (((now + tmp)%n)+n)%n;
                cout << now << " ";
                if(now == pre){
                    break;
                }
                if(now == i){
                    return true;
                }
                if(nums[now] > 1000){
                    tmp = nums[now];
                    int r = 0;
                    while(tmp > 1000){
                        r ++ ;
                        tmp -= 2000;
                    }
                    if(round == r){
                        return true;
                    }
                }
                
            }
            nums[i] += MAXN;
            cout << endl;
        }
        return false;
    }
};
```

```c++
class Solution {
public:
    bool circularArrayLoop(vector<int>& nums) {
        //vector<int> is_checked(nums.size(), 0);
        int size = nums.size();
        
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 0) continue;
            
            int slow, fast, diff;
            slow = i;
            fast = i;
            diff = 0;
            
            while (1) {
                bool invalid = false;
                // move fast two step foward
                for (int c = 0; c < 2; c++) {
                    int step = nums[fast];
                    // check direction 
                    if (diff != 0 && (step * diff) < 0) {
                        invalid = true;
                        break;
                    }
                    diff = step;
                    // move
                    int last = fast;
                    fast = getIndex(fast + step, size);   
                    
                    // Check if the length of the cycle is 1
                    if (fast == last) {
                        invalid = true;
                        break;
                    }
                    //is_checked[last] = 1;
                }                
                
                // Set the value (before meet the opposite value) on this path to 0
                if (invalid) {
                    slow = i;
                    int last = nums[slow];
                    while (last * nums[slow] > 0) {
                        int tmp = nums[slow];
                        nums[slow] = 0;
                        slow = getIndex(slow + tmp, size);
                    }
                    break;
                }
                
                // move slow one step
                slow = getIndex(slow + nums[slow], size);
                if (slow == fast) return true;
            }
        }
        return false;
    }
    int getIndex(int a, int b) {
        a %= b;
        return (a < 0)? a+b:a;
    }
};
```