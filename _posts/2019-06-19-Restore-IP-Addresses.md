---
layout: post
title:  "93. Restore IP Addresses"
date: 2019-06-19 20:37:00 -0400
categories: articles
---
Given a string containing only digits, restore it by returning all possible valid IP address combinations.

Example:
```
Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]
```
```c++
//2019-07-10
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> res;
        if (s.size() > 12 || s.size() < 4) return res;
        helper(s, 0, 0, "", res);
        return res;
    }
    void helper(string s, int idx, int sect, string tmp, vector<string>& res){
        if ( idx == s.size() && sect == 4 ) {
            res.push_back(tmp);
        } else {
            for ( int i = idx; i < s.size(); i++ ) {
                if ( sect < 4 && i-idx < 3 && Valid(s, idx, i)){
                    tmp += s.substr(idx, i - idx + 1);
                    sect++;
                    if ( sect < 4 ) tmp.push_back('.');
                    helper(s, i+1, sect, tmp, res);
                    if ( sect < 4 ) tmp.pop_back();
                    sect--;
                    for(int j = 0; j < i - idx + 1; j++) tmp.pop_back();
                }
            }
        }
    }
    bool Valid(string s, int start, int end){
        string temp = s.substr(start, end-start+1);
        int ip = stoi(temp);
        if(s[start] == '0' && start != end) return false; // leading 0
        else if(ip >= 0 && ip <= 255) return true;
        return false;
    }
};
```
```c++
class Solution {
public:
    vector<string> result;
    string solution;
    
    vector<string> restoreIpAddresses(string s) {
        
        backtracking(s, 0, 0);
        
        return result;
    }
    
private:
    void backtracking(string s, int start, int part)
    {
        if(start == s.size() && part == 4)
        {
            result.push_back(solution);
            return;
        }
        
        for(int i = start; i < s.size(); i++)
        {
            if(part < 4 && i-start < 3 && validIP(s, start, i))
            {
                solution.append(s.substr(start, i-start+1));
                part++;
                if(part < 4) solution.push_back('.');

                backtracking(s, i+1, part);
                
                if(part < 4) solution.pop_back();
                part--;
                for(int j = 0; j < i-start+1; j++) solution.pop_back();
            }
        }
    }
    
    bool validIP(string s, int start, int end)
    {
        string temp = s.substr(start, end-start+1);
        int ip = stoll(temp);
        
        if(s[start] == '0' && start != end) return false;
        else if(ip >= 0 && ip <= 255) return true;
        
        return false;
    }
};
```
```c++
class Solution {
public:
    vector<string> restoreIpAddresses(string s) 
    {
        vector<string> reslt; 
        vector<string> ip; 
        
        restorIP_helper(s, reslt, ip, 0); 
        return reslt; 
    }
    
    
    void restorIP_helper(string s, vector<string> & reslt, vector<string> & ip, int start)
    {
        if (ip.size() == 4 && start == s.length())
        {
            string topush = Stringalize(ip);
            if (!topush.empty())
                reslt.push_back(topush); 
            return ;
        }
        
        if (s.size() - start > (4 - ip.size()) * 3) return; 
        if (s.size() - start < (4 - ip.size())) return;
        
        int num = 0; 
        
        for (int i = start; i < start + 3 && i < s.length(); i++)
        {
            num = num * 10 + (s[i] - '0'); 
            if (num <= 255 && num >= 0)
            {
                ip.push_back(s.substr(start, i - start + 1)); 
                restorIP_helper(s, reslt, ip, i + 1); 
                ip.pop_back() ;
            }
            
            //if (num == 0) break;
        }
        
    }
    
    string Stringalize(vector<string> ip)
    {
        string topush; 
        for (auto x : ip)
            if (x[0] == '0' && x.length() > 1) return topush; 
        return ip[0] + '.' + ip[1] + '.' + ip[2] + '.'+ ip[3];
    }
    
    
};
```
