---
layout: post
title:  "517. Super Washing Machines"
date: 2019-08-07 20:15:00 -0400
categories: articles
---
You have n super washing machines on a line. Initially, each washing machine has some dresses or is empty.

For each move, you could choose any m (1 ≤ m ≤ n) washing machines, and pass one dress of each washing machine to one of its adjacent washing machines at the same time .

Given an integer array representing the number of dresses in each washing machine from left to right on the line, you should find the minimum number of moves to make all the washing machines have the same number of dresses. If it is not possible to do it, return -1.

Example1
```
Input: [1,0,5]

Output: 3

Explanation: 
1st move:    1     0 <-- 5    =>    1     1     4
2nd move:    1 <-- 1 <-- 4    =>    2     1     3    
3rd move:    2     1 <-- 3    =>    2     2     2   
```
Example2
```
Input: [0,3,0]

Output: 2

Explanation: 
1st move:    0 <-- 3     0    =>    1     2     0    
2nd move:    1     2 --> 0    =>    1     1     1     
```
Example3
```
Input: [0,2,0]

Output: -1

Explanation: 
It's impossible to make all the three washing machines have the same number of dresses. 
Note:
The range of n is [1, 10000].
The range of dresses number in a super washing machine is [0, 1e5].
```

```c++
class Solution {
public:
    int findMinMoves(vector<int>& machines) {
        int sum = accumulate(machines.begin(), machines.end(), 0);
        if(sum%machines.size() != 0)
            return -1;
        int res = 0, count = 0, avg = sum/machines.size();
        for(auto a : machines){
            count += a - avg;
            res = max(res, max(abs(count), a - avg));
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int findMinMoves(vector<int>& m) {
        int sum = accumulate(m.begin(), m.end(), 0);
        int n = m.size();
        if(sum%n!=0) return -1;
        sum /= n;
        int ans = 0, temp = 0;
        for(int i = 0; i<n; i++){
            temp += m[i];
            
            ans = max(ans, abs(temp - (i+1)*sum));
            
            ans = max(ans, m[i] - sum);
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
int findMinMoves(vector<int>& machines) {
    int totalDresses = 0, size = machines.size();
    for (auto i = 0; i < size; ++i) totalDresses += machines[i];
    if (totalDresses % size != 0) return -1;
    
    auto targetDresses = totalDresses / size, totalMoves = 0, ballance = 0;
    for (auto i = 0; i < size; ++i) {
        ballance += machines[i] - targetDresses;
        totalMoves = max(totalMoves, max(machines[i] - targetDresses, abs(ballance)));
    }
    return totalMoves;
}
};
```
```c++
class Solution {
public:
    int findMinMoves(vector<int>& machines) {
        int sum=0;
        for(int i=0;i<machines.size();i++){
            sum+=machines[i];
        }
        if(sum%machines.size()) return -1;
        int target=sum/machines.size(),toRight=0,res=0;
        for(int m: machines){
            toRight=m+toRight-target;
            res=max(res,max(abs(toRight),m-target));
        }
        return res;
    }
};
```