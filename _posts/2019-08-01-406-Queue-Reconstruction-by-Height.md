---
layout: post
title:  "406. Queue Reconstruction by Height"
date: 2019-08-01 20:21:00 -0400
categories: articles
---
Suppose you have a random list of people standing in a queue. Each person is described by a pair of integers (h, k), where h is the height of the person and k is the number of people in front of this person who have a height greater than or equal to h. Write an algorithm to reconstruct the queue.

Note:
The number of people is less than 1,100.
Example
```
Input:
[[7,0], [4,4], [7,1], [5,0], [6,1], [5,2]]

Output:
[[5,0], [7,0], [5,2], [6,1], [4,4], [7,1]]
```
```c++
class Solution {
public:

  vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
    auto cmp = [](const vector<int> &a, const vector<int> &b) {
            return a[0] > b[0] || (a[0] == b[0] && a[1] < b[1]);
        };
    sort(people.begin(), people.end(), cmp);
    vector<vector<int>> res;
    for(auto p:people) res.insert(res.begin()+p[1],p);
    return res;
  }
};
```
```c++
class Solution {
private:
    class compare{
    public:
    bool operator()(vector<int>& x1, vector<int>& x2){
        return x1[0]<x2[0]||x1[0]==x2[0]&&x1[1]>x2[1];}
    };
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        std::sort(people.begin(), people.end(), compare());
        vector<vector<int>> result(people.size(), vector<int>(2,0));
        //set<int> index;
        vector<int> index(people.size(), 0);
        for(int i = 0; i<people.size();i++) index[i] = i;
        for(int i = 0; i<people.size();i++){
            result[index[people[i][1]]]=people[i];
            index.erase(index.begin()+people[i][1]);
        }
        return result;
    }
};
```
```c++
class Solution {
public:
vector<pair<int, int>> reconstructQueue(vector<pair<int, int>>& people) {
    auto comp = [](const pair<int, int>& p1, const pair<int, int>& p2)
                    { return p1.first > p2.first || (p1.first == p2.first && p1.second < p2.second); };
    sort(people.begin(), people.end(), comp);
    vector<pair<int, int>> res;
    for (auto& p : people) 
        res.insert(res.begin() + p.second, p);
    return res;
}
};
```