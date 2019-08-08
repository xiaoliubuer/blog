---
layout: post
title:  "527. Word Abbreviation"
date: 2019-08-07 20:50:00 -0400
categories: articles
---
Given an array of n distinct non-empty strings, you need to generate minimal possible abbreviations for every word following rules below.
```
Begin with the first character and then the number of characters abbreviated, which followed by the last character.
If there are any conflict, that is more than one words share the same abbreviation, a longer prefix is used instead of only the first character until making the map from word to abbreviation become unique. In other words, a final abbreviation cannot map to more than one original words.
If the abbreviation doesn't make the word shorter, then keep it as original.
```
Example:
```
Input: ["like", "god", "internal", "me", "internet", "interval", "intension", "face", "intrusion"]
Output: ["l2e","god","internal","me","i6t","interval","inte4n","f2e","intr4n"]
```
Note:
```
Both n and the length of each word will not exceed 400.
The length of each word is greater than 1.
The words consist of lowercase English letters only.
The return answers should be in the same order as the original array.
```
```c++
class Solution {
public:
    vector<string> wordsAbbreviation(vector<string>& dict) {
        vector<string> res(dict.size());
        unordered_map<string, vector<int>> category;
        for (int i = 0; i < dict.size(); i++) {
            string id = dict[i].substr(0, 1) + dict[i].back() + to_string(dict[i].size());
            category[id].push_back(i);
        }
        for (const auto& [id, v] : category) {
            Trie tr;
            for (int i : v) tr.add(dict[i]);
            for (int i : v) res[i] = tr.getAbbr(dict[i]);
        }
        return res;
    }

    class Trie {
    public:
        struct Node {
            Node* child[26] = { nullptr };
            int count = 0;
        };

        void add(const string& s) {
            auto p = root;
            for (int i = 0; i < s.size() - 1; i++) {
                int index = s[i] - 'a';
                if (!p->child[index]) p->child[index] = new Node();
                p = p->child[index];
                p->count++;
            }
        }

        string getAbbr(const string& s) {
            string res;
            auto p = root;
            for (int i = 0; i < s.size() - 1; i++) {
                p = p->child[s[i] - 'a'];
                res += s[i];
                if (p->count == 1) break;
            }
            int nonAbbrLen = s.size() - res.size() - 1;
            if (nonAbbrLen <= 1) return s;
            return res + to_string(nonAbbrLen) + s.back();
        }

        Node* root = new Node();
    };
};
```
```c++
class Solution {
    unordered_map<string, int> pos;
public:
    vector<string> wordsAbbreviation(vector<string>& dict) {
        for(int i = 0; i<dict.size(); i++){
            resolve(i, dict);
        }
        vector<string> abbreviationVec(dict.size(), "");
        for(auto [comp, place]: pos)
            if(place>-1) 
                abbreviationVec[place] = comp;
        return abbreviationVec;
    }
    
    string abbreviate(string& s, int lead){
        string compressed = s.substr(0, lead) + to_string(s.length() - 1 - lead) + s.back();
        if(compressed.length()<s.length()) return compressed;
        return s;
    }
    
    void resolve(int i, vector<string>& dict, int lead = 1){
        string comp = abbreviate(dict[i], lead);
        if(pos.find(comp) == pos.end()){
            pos[comp] = i;
        } else {
            if(pos[comp] == -1) resolve(i, dict, lead + 1);
            else resolve(i, pos[comp], dict, lead+1);
            pos[comp] = -1;
        }
    }
    
    void resolve(int i, int j, vector<string>& dict, int lead){
        // i, j are valid indices
        // cout<<i<<" "<<j<<" "<<lead<<endl;
        string comp1 = abbreviate(dict[i], lead), comp2 = abbreviate(dict[j], lead);
        if(comp1 == comp2){
            pos[comp1] = -1;
            resolve(i, j, dict, lead+1);
        } else {
            pos[comp1] = i;
            pos[comp2] = j;
        }
    }
    
};
```

```c++
class Solution {
public:
    vector<string> wordsAbbreviation(vector<string>& dict) {
        unordered_map<string, vector<int>> M;
        vector<string> rtn(dict.size());
        for (int i = 0 ; i < dict.size() ; ++i) {
            string key = dict[i][0] + to_string(dict[i].size()-2) + dict[i].back();
            if (key.size() < dict[i].size())
                M[key].push_back(i);
            else
                rtn[i] = dict[i];
        }
                
        for (auto &p : M) {
            if (p.second.size() > 1)
                revolseConflict(dict, p.second, rtn);
            else
                rtn[p.second[0]] = p.first;
        }
        return rtn;
    }
    
    void revolseConflict(vector<string>& dict, vector<int> &v, vector<string> &rtn) {
        int len = dict[v[0]].size();
        vector<string> conflict;
        for (int i = 0 ; i < v.size() ; ++i) conflict.push_back(dict[v[i]]);
        
        sort(conflict.begin(), conflict.end());
       
        unordered_map<string, string> M;        
        int prefix = 0;
        for (int i = 1 ; i <= conflict.size() ; ++i) {
            int j = 0;
            
            while (i < conflict.size() && j < len && conflict[i][j] == conflict[i-1][j]) ++j;
            
            if (j > prefix)
                prefix = j;
                
            if (prefix + 2 + to_string(len - prefix - 2).size() < len) {
                string abbr = conflict[i-1].substr(0, prefix+1) + to_string(len - prefix - 2) + conflict[i-1].back();
                M[conflict[i-1]] = abbr;                
            }            
            prefix = j;
        }
        for (int i : v) {
            if (M.count(dict[i])) rtn[i] = M[dict[i]];
            else rtn[i] = dict[i];
        }
    }
};
```
```c++
class Solution{
public:
    vector<string> wordsAbbreviation(vector<string>& dict) {
        int n = dict.size();
        vector<string> ans = dict;
        // sort the dict
        sort(dict.begin(), dict.end(), mycompare);
        unordered_map<string, string> mp;
        // prefix is the longest common prefix between dict[i] and dict[i-1]
        int prefix = 0; 
        for (int i = 0; i < n; ++i) {
            int j = 0;
            if (i < n-1 && dict[i].size() == dict[i+1].size() && dict[i].back() == dict[i+1].back()) {
                while (j < dict[i].size() && dict[i][j] == dict[i+1][j])
                    j++;
            }
            cout << dict[i] << endl;
            cout << j << endl;
            if (j > prefix) prefix = j;
            
            // build abbreviation if it is shorter than word, and put it in a map
            if (dict[i].size() > prefix+3) {
                string s = dict[i].substr(0, prefix+1)+to_string(dict[i].size()-prefix-2)+dict[i].back();
                mp[dict[i]] = s;
            }
            // update prefix to be longest prefix with previous word
            prefix = j;
        }
        for (int i = 0; i < n; ++i) {
            if (mp.count(ans[i])) ans[i] = mp[ans[i]];
        }
        return ans;
    }
private:
    static bool mycompare(string& a, string& b) {
        if (a.size() == b.size()) {
            if (a.back() < b.back()) 
                return true;
            else if (a.back() > b.back()) 
                return false;
            for (int i = 0; i < a.size()-1; ++i) {
                if (a[i] < b[i]) 
                    return true;
                else if (a[i] > b[i])
                    return false;
            }
        }
        return a.size() < b.size();
    }
};
```