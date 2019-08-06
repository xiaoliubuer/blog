---
layout: post
title:  "455. Assign Cookies"
date: 2019-08-05 19:09:00 -0400
categories: articles
---
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor gi, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size sj. If sj >= gi, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

Note:
You may assume the greed factor is always positive. 
You cannot assign more than one cookie to one child.

Example 1:
```
Input: [1,2,3], [1,1]

Output: 1
```
Explanation: You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
You need to output 1.

Example 2:
```
Input: [1,2], [1,2,3]

Output: 2
```
Explanation: You have 2 children and 3 cookies. The greed factors of 2 children are 1, 2. 
You have 3 cookies and their sizes are big enough to gratify all of the children, 
You need to output 2.

```c++
class Solution {
public:
	int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        int greedIndex = 0;
        int cookieIndex = 0;
        int count = 0;
        while(greedIndex < g.size() && cookieIndex < s.size()){
            //give cookie to child since they will be content with it
            if(s.at(cookieIndex) >= g.at(greedIndex)){
                count++;
                cookieIndex++;
                greedIndex++;
            }
            //keep looking for cookie to content current child
            else{
                cookieIndex++;
                continue;
            }
        }
        return count;
}
};
```
```c++
class Solution {
public:
	int findContentChildren(vector<int>& g, vector<int>& s) {
		sort(g.begin(),g.end());
		sort(s.begin(),s.end());            
		int ans=0;
		int p1=0; int p2=0;
		while(p1<g.size() && p2<s.size()){
			if(s[p2]>=g[p1]){
				ans++;
				p1++;
				p2++;
			}else if(s[p2]<g[p1]){
				p2++;
			}
		}
		return ans;

	}
};
```