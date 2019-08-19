---
layout: post
title:  "616. Add Bold Tag in String"
date: 2019-08-15 08:11:00 -0400
categories: articles
---	
Given a string s and a list of strings dict, you need to add a closed pair of bold tag <b> and </b> to wrap the substrings in s that exist in dict. If two such substrings overlap, you need to wrap them together by only one pair of closed bold tag. Also, if two substrings wrapped by bold tags are consecutive, you need to combine them.
Example 1:
```
Input: 
s = "abcxyz123"
dict = ["abc","123"]
Output:
"<b>abc</b>xyz<b>123</b>"
```
Example 2:
```
Input: 
s = "aaabbcc"
dict = ["aaa","aab","bc"]
Output:
"<b>aaabbc</b>c"
```
Note:
```
The given dict won't contain duplicates, and its length won't exceed 100.
All the strings in input have length in range [1, 1000].
```
```c++
class Solution {
public:
    string addBoldTag(string s, vector<string>& dict) 
    {
        if(s.size() == 0)
            return s;
        vector<bool> bold(s.size());
        for(auto word: dict)
        {
            int wordlen = word.length();
            // for each word in dictionary, use Sunday algorithm to tag the place where it is.
            unordered_map<string, int> movelength;
            for(int i = 0; i < word.length(); i++)
            {
                if(movelength.find(word.substr(i, 1)) != movelength.end())
                    movelength[word.substr(i,1)] = word.length() - i;
                else
                    movelength.insert({word.substr(i,1), word.length() - i});
            }
            int i = 0;
            while(i < s.length())
            {
                int j = 0;
                for(; j < wordlen && (i + j < s.length()); ++j)
                {
                    if(s[i+j] != word[j])
                        break;
                }
                if(j >= word.length())
                {
                    fill(bold.begin()+i, bold.begin()+i+wordlen, true);
                }
                if(i + word.length() >= s.length())
                    break;
                if(movelength.find(s.substr(i+wordlen, 1)) != movelength.end())
                    i += movelength.find(s.substr(i+wordlen, 1))->second;
                else
                    i += wordlen + 1;
            }
        }
        string ret;
        bool state = true;
        
        // find each bold word
        for(int i = 0; i < s.size(); i++)
        {
            if(state && bold[i])
            {
                ret += "<b>";
                state = false;
            }
            else if(!state && !bold[i])
            {
                ret += "</b>";
                state = true;
            }
            ret.push_back(s[i]);
        }
        if(!state)
            ret += "</b>";
        return ret;
    }
};
```
```c++
class Solution {
public:
string addBoldTag(string s, vector<string>& dict) {
    if (s.empty()) return "";
    typedef pair<int,int> pi;
    map<int,int> tagPos;
    for (string& word : dict) {
        int wordStart = -1, wordEnd = 0;
        while (true) {
            wordStart = (int)s.find(word, wordStart+1);
            if (wordStart == -1) break;                                
            wordEnd = wordStart + word.size();

            auto ins = tagPos.insert(pi(wordStart, wordEnd));
            if (!ins.second) tagPos[wordStart] = max(wordEnd, tagPos[wordStart]);
            auto mid = ins.first;
            auto right = next(mid);
            auto left = mid == tagPos.begin() ? tagPos.end() : prev(mid);
            
            if (left != tagPos.end() && left->second >= wordStart) {
                swap(mid, left);
                mid->second = max(left->second, mid->second);
                tagPos.erase(left);
            }
                            
            while (right != tagPos.end() && right->first <= mid->second) {
                mid->second = max(right->second, mid->second);
                right = next(right);
                tagPos.erase(prev(right));
            }                
        }
    }
    
    string res; 
    cout << tagPos.size() << endl;
    int start = 0;
    for (auto p : tagPos) {
        res += s.substr(start, p.first-start);
        res += "<b>";
        res += s.substr(p.first, p.second - p.first);
        res += "</b>";
        start = p.second;
    }
    if (start < s.size()) res += s.substr(start);
    return res;
}
};
```