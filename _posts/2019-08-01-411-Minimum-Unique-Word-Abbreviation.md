---
layout: post
title:  "411. Minimum Unique Word Abbreviation"
date: 2019-08-01 20:40:00 -0400
categories: articles
---
A string such as "word" contains the following abbreviations:

["word", "1ord", "w1rd", "wo1d", "wor1", "2rd", "w2d", "wo2", "1o1d", "1or1", "w1r1", "1o2", "2r1", "3d", "w3", "4"]
Given a target string and a set of strings in a dictionary, find an abbreviation of this target string with the smallest possible length such that it does not conflict with abbreviations of the strings in the dictionary.

Each number or letter in the abbreviation is considered length = 1. For example, the abbreviation "a32bc" has length = 4.

Note:
```
In the case of multiple answers as shown in the second example below, you may return any one of them.
Assume length of target string = m, and dictionary size = n. You may assume that m ≤ 21, n ≤ 1000, and log2(n) + m ≤ 20.
```
Examples:
```
"apple", ["blade"] -> "a4" (because "5" or "4e" conflicts with "blade")

"apple", ["plain", "amber", "blade"] -> "1p3" (other valid answers include "ap3", "a3e", "2p2", "3le", "3l1").
```
```c++
class Solution {
public:
// return the abbreviation length, bits of 1 represent
// bits to abbreviate
int abbrLen(int num, int n) {
    int count = 0;
    int i = n-1;
    while (i>=0) {
        if (num & 1<<i) {
            ++count;
            while (i>=0 && (num & 1<<i)) {
                --i;
            }
        } else {
            ++count;
            --i;
        }
    }
    return count;
}

void backTrack(int& result,
               int& resultLen,
               std::vector<int>& bitsToTest,
               int idx,
               std::vector<int>& bits,
               int current,
               int n)
{
    if (idx == bitsToTest.size()) {
        int len = abbrLen(current, n);
        if (len < resultLen) {
            resultLen = len;
            result = current;
        }
        return; 
    }

    int testBit = bitsToTest[idx];
    
    // option 1, leave this test bit as 0
    backTrack(result, resultLen, bitsToTest, idx+1, bits, current, n);

    // option 2, set this test bit as 1
    int newCurrent = current | 1<<testBit;

    // check if it is valid
    bool valid = true;
    for (auto bit : bits) {
        if ((newCurrent & bit) == bit) {
            valid = false;
            break;
        }
    }

    if (valid) {
        backTrack(result, resultLen, bitsToTest, idx+1, bits, newCurrent, n);
    }
}

std::string minAbbreviation(std::string target,
                            std::vector<std::string>& dictionary) {
    int n = target.size();
    std::vector<int> bits;
    for (auto& word : dictionary) {
        if (word.size() == n) {
            int num = 0;
            for (int i=0; i<n; ++i) {
                if (word[i] != target[i]) {
                    num = num | 1<<i;
                }
            }
            bits.push_back(num);
        }
    }

    // if no word of same size, then we can abbreviate every
    // char of the target
    if (bits.empty()) {
        return std::to_string(n);
    }

    // find which bits to abbreviate 
    int num = 0;
    for (auto bit : bits) {
        num = num | bit;
    }

    // these are the bits we want to test which of them we can
    // abbreviate, for the rest, we always abbreviate
    std::vector<int> bitsToTest;
    for (int i=0; i<n; ++i) {
        if (num & 1<<i) {
            bitsToTest.push_back(i);
        }
    }

    // flip every bit of num, now 1 represent abbreviate, 0
    // represent do not abbreviate
    num = ~num;
    int len = n;

    // backtrack to find shortest
    backTrack(num, len, bitsToTest, 0, bits, num, n);

    // construct string from number
    std::string result;
    int i=0;
    while (i<n) {
        if (num & 1<<i) {
            int count = 0;
            while (i<n && (num & 1<<i)) {
                ++i;
                ++count;
            }
            result += std::to_string(count);
        } else {
            result.push_back(target[i]);
            ++i;
        }
    }

    return result;
}

};
```
```c++
class Solution {
public:
    string minAbbreviation(string target, vector<string>& dictionary) {
        string res = "";
        bool found = false;
        for(int i = 0; i <= target.size() && !found; i++) DFS(dictionary, i, 0, target, res, found, vector<int>());
        return res;
    }
    // k: number of letters to pick
    // vec: indices of letters picked
    void DFS(vector<string>& dictionary, int k, int pos, string target, string& res, bool& found, vector<int> vec){
        if(k == 0){  
            int i = 0;
            for(; i < dictionary.size(); i++){
                if(dictionary[i].size() != target.size()) continue;  // word of different length won't cause conflict
                int count = 0;
                for(int x: vec) if(target[x] == dictionary[i][x]) count++;
                if(vec.empty() || count == vec.size()) break;  // encounter a conflict word
            }
            if(i == dictionary.size()){
                found = true;
                if(vec.empty()){
                    res = to_string(target.size());
                    return;
                }
                // translate 'vec' into abbreviation(string)
                string s = "";
                sort(vec.begin(), vec.end());
                for(int j = 0; j < vec.size(); s += target[vec[j]], j++)
                    if(j == 0) s += (vec[0] == 0) ? "" : to_string(vec[0]);
                    else s += (vec[j] - vec[j - 1] - 1 == 0) ? "" : to_string(vec[j] - vec[j - 1] - 1);
                if(vec.back() != target.size() - 1) s += to_string(target.size() - vec.back() - 1);
                if(res.empty() || length(res) > length(s)) res = s;
            }
            return;
        }
        if(pos == target.size()) return;
        DFS(dictionary, k, pos + 1, target, res, found, vec);
        vec.push_back(pos);
        DFS(dictionary, k - 1, pos + 1, target, res, found, vec);
    }
    
    int length(string& s){
        int len = 0;
        for(int i = 0; i < s.size(); i++, len++)
            if(isdigit(s[i])) while(i + 1 < s.size() && isdigit(s[i + 1])) i++;
        return len;
    }
};
```