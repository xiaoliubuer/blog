---
layout: post
title:  "682. Baseball Game"
date: 2019-08-18 17:36:00 -0400
categories: articles
---
You're now a baseball game point recorder.

Given a list of strings, each string can be one of the 4 following types:

Integer (one round's score): Directly represents the number of points you get in this round.
"+" (one round's score): Represents that the points you get in this round are the sum of the last two valid round's points.
"D" (one round's score): Represents that the points you get in this round are the doubled data of the last valid round's points.
"C" (an operation, which isn't a round's score): Represents the last valid round's points you get were invalid and should be removed.
Each round's operation is permanent and could have an impact on the round before and the round after.

You need to return the sum of the points you could get in all the rounds.

Example 1:
```
Input: ["5","2","C","D","+"]
Output: 30
```
Explanation: 
```
Round 1: You could get 5 points. The sum is: 5.
Round 2: You could get 2 points. The sum is: 7.
Operation 1: The round 2's data was invalid. The sum is: 5.  
Round 3: You could get 10 points (the round 2's data has been removed). The sum is: 15.
Round 4: You could get 5 + 10 = 15 points. The sum is: 30.
```
Example 2:
```
Input: ["5","-2","4","C","D","9","+","+"]
Output: 27
```
Explanation: 
```
Round 1: You could get 5 points. The sum is: 5.
Round 2: You could get -2 points. The sum is: 3.
Round 3: You could get 4 points. The sum is: 7.
Operation 1: The round 3's data is invalid. The sum is: 3.  
Round 4: You could get -4 points (the round 3's data has been removed). The sum is: -1.
Round 5: You could get 9 points. The sum is: 8.
Round 6: You could get -4 + 9 = 5 points. The sum is 13.
Round 7: You could get 9 + 5 = 14 points. The sum is 27.
```
Note:
```
The size of the input list will be between 1 and 1000.
Every integer represented in the list will be between -30000 and 30000.
```
```c++
class Solution {
public:
    int calPoints(vector<string>& ops)
	{
        vector<int> A;
        for (auto& op: ops)
        {
            auto n = A.size();
            
            if (     op == "C") A.pop_back();
            else if (op == "D") A.push_back(2 * A.back());
            else if (op == "+") A.push_back(A[n - 2] + A[n - 1]);
            else                A.push_back(stoi(op));   
        }
        return accumulate(A.begin(), A.end(), 0);
    }
};
```
```c++
class Solution {
public:
   int calPoints( vector<string>& ops, vector<int> A={} )
	{
        for ( auto& op: ops )
        {
            auto N = A.size();
            
            if(      op == "C" ) A.pop_back();
            else if( op == "D" ) A.push_back( 2 * A.back() );
            else if( op == "+" ) A.push_back( A[ N-2 ] + A[ N-1 ] );
            else                 A.push_back( stoi( op ) );   
        }
        return accumulate( A.begin(), A.end(), 0 );
    }
};
```
```c++
class Solution {
public:
    int calPoints(vector<string>& ops) {
        stack <int> s;
        for (string ch: ops) {
            if (ch == "C") {
                s.pop();
            } else if (ch == "D") {
                s.push(s.top() * 2);
            } else if (ch == "+") {
                int a = s.top(); s.pop();
                int b = s.top(); s.push(a);
                int c = a + b;
                s.push(c);
            } else {
                s.push(stoi(ch));
            }
        }
        int res = 0;
        while (!s.empty()) {
            res += s.top();
            s.pop();
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    int calPoints(vector<string>& ops) {
    	vector<int> input_cache;
    	for(auto t : ops){
    		if ( t == "+"){
    			int top = input_cache.back();
    			input_cache.pop_back();
    			int newtop = top + input_cache.back(); 
    			input_cache.push_back(top);
    			input_cache.push_back(newtop);
    		}
    		else if( t == "C"){
    			input_cache.pop_back();
    		}
    		else if (t == "D"){
    			input_cache.push_back( 2 * input_cache.back());
    		}
    		else
    			input_cache.push_back(stoi(t));
    	}
        
        int ans = 0;
        for (auto i : input_cache) ans += i;
        return ans;
    }

};
```