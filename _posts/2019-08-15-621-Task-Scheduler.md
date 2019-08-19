---
layout: post
title:  "621. Task Scheduler"
date: 2019-08-15 08:15:00 -0400
categories: articles
---	

Given a char array representing tasks CPU need to do. It contains capital letters A to Z where different letters represent different tasks. Tasks could be done without original order. Each task could be done in one interval. For each interval, CPU could finish one task or just be idle.

However, there is a non-negative cooling interval n that means between two same tasks, there must be at least n intervals that CPU are doing different tasks or just be idle.

You need to return the least number of intervals the CPU will take to finish all the given tasks.

 

Example:
```
Input: tasks = ["A","A","A","B","B","B"], n = 2
Output: 8
```
Explanation: A -> B -> idle -> A -> B -> idle -> A -> B.

Note:
```
The number of tasks is in the range [1, 10000].
The integer n is in the range [0, 100].
```
```c++
class Solution {
public:
int leastInterval(vector<char>& tasks, int n) {
	vector<int> count(26);
	int hi = 0, hiCount = 0;
	for(char t : tasks) hi = max(hi, ++count[t - 'A']);
	for(int i = 0; i < 26; i++) if(count[i] == hi) hiCount++;
	return max((int)tasks.size(), (hi - 1) * (n + 1) + hiCount);
}
};
```
```c++
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
    
        ios_base::sync_with_stdio(false);
        
        vector<int> freq(26);
        
        for(int i = 0;i<tasks.size();i++){
            
            freq[tasks[i]-'A']++;
        }
        
        sort(freq.begin(),freq.end(),[&](int p1, int p2){
            
            if(p1>p2)
                return true;    
            return false;
        });
        
        queue<pair<int,int>> q;
        
        for(int i = 0;i<freq.size();i++){
            
            if(freq[i]>0){
                q.push({freq[i], i});
                
            }else{
                break;
            }    
        }
        
        int cnt = 0,interval = n,index = 0;
        
        while(!q.empty()){
            
            if(q.front().second<=cnt){

                if(q.front().first-1>0){
                    q.front().first -= 1;
                    q.front().second += n+1;
                    q.push(q.front());
                }
                q.pop();
            }
            cnt++;
        }
        
        return cnt;
    }
};
```
```c++
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        vector<int> charCount(26,0);
        int max = 0;
        int maxChars = 0;

        for(char c : tasks) {
            int tmp = ++charCount[c-'A'];
            if(tmp == max) 
                maxChars++;
            if(tmp > max){
                max = tmp; 
                maxChars = 1;
            } 
        }
        
        int empty = (max-1) * (n-maxChars+1);
        int remaining = tasks.size() - (max*maxChars);
        int idles = std::max(0, empty-remaining);
        
        return tasks.size() + idles;
        
    }
};
```

```c++
class Solution
{
 int leastInterval (vector<char>const& tasks, int const n)
 {
  if (tasks.empty()) { return 0; }

  int counts [26] {};
  for (auto const c : tasks)
  {
   ++counts[c-'A'];
  }
  sort(begin(counts),end(counts),greater<>{});

  int const longest = counts[0];
  int idleslots = (longest-1) * n;
  for (int i = 1; i < 26 && counts[i] && idleslots > 0; ++i)
  {
   idleslots -= counts[i] - (counts[i] == longest);
  }

  return tasks.size() + max(0,idleslots);
 }
};
```