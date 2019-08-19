---
layout: post
title:  "648. Replace Words"
date: 2019-08-18 15:39:00 -0400
categories: articles
---
In English, we have a concept called root, which can be followed by some other words to form another longer word - let's call this word successor. For example, the root an, followed by other, which can form another word another.

Now, given a dictionary consisting of many roots and a sentence. You need to replace all the successor in the sentence with the root forming it. If a successor has many roots can form it, replace it with the root with the shortest length.

You need to output the sentence after the replacement.

Example 1:
```
Input: dict = ["cat", "bat", "rat"]
sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
```
Note:
```
The input will only have lower-case letters.
1 <= dict words number <= 1000
1 <= sentence words number <= 1000
1 <= root length <= 100
1 <= sentence words length <= 1000
```

```c++
class TrieNode {
public:
    bool isend;
    string word;
    TrieNode* children[26];
    TrieNode() {
        isend = false;
        memset(children, NULL, sizeof(children));
        word = "";
    }
};

class Trie {
public:
    TrieNode* root;
    Trie() {
        root = new TrieNode();
    }
    
    void insert(string word) {
        TrieNode* node = root;
        for (char c : word) {
            if (!node->children[c - 'a'])
                node->children[c - 'a'] = new TrieNode();
            node = node->children[c - 'a'];
        }
        node->isend = true;
        node->word = word;
    }
    
    string searchroot(string word) {
        TrieNode* node = root;
        int l = word.size(), i = 0;
        for (; i < l && node; i++) {
            node = node->children[word[i] - 'a'];
            if (node && node->isend) return node->word;
        }
        return word;
    }
};


class Solution {
public:
    string replaceWords(vector<string>& dict, string sentence) {
        string res;
        Trie trie;
        for (string& rootword : dict) 
            trie.insert(rootword);
        stringstream ss(sentence);
        string tmp;
        while(getline(ss, tmp, ' ')) {
            res += trie.searchroot(tmp) + ' ';
        }
        res.pop_back();
        return res;
    }
};
```
```c++
class trie {
private:
    bool is_root = false;
    trie *arr[26] = {};
public:
    void insert(std::string &word, int ch, int size){
        is_root |= ch == size;
        if(!is_root){
            if(arr[word[ch] - 'a'] == nullptr)
                arr[word[ch] - 'a'] = new trie();
            arr[word[ch] - 'a']->insert(word, ch + 1, size);
        }
    }
    
    int root(std::string &word, int st, int ch, int size){
        if(st + ch == size || word[st + ch] == ' ' || this->is_root)
            return ch;
        if(arr[word[st + ch] - 'a'] == nullptr){
            while(st + ch < size && word[st + ch] != ' ')
                ch++;
            return ch;
        }
        return arr[word[st + ch] - 'a']->root(word, st, ch + 1, size);
    }
};

class Solution {
public:
    std::string replaceWords(std::vector<string>& dict, std::string sentence) {
        trie t;
        std::string res;
        const int n = sentence.size();
        
        for(auto s : dict)
            t.insert(s, 0, s.size());
        for(int i = 0; i < n;){
            if(sentence[i] == ' ')
                res += sentence[i++];
            auto chars  = t.root(sentence, i, 0, n);
            res += sentence.substr(i, chars);
            for(i += chars; i < n && sentence[i] != ' '; i++);
        }
        return res;
    }
private:
};
```
```c++
class Solution {
    struct Node {
        bool isRoot = false;
        Node* next[26];
    };
    Node* head;
public:
    void insert(string word) {
        Node* root = head;
        for (char c: word) {
            if (root->next[c-'a'] == NULL) root->next[c-'a'] = new Node();
            root = root->next[c-'a'];
        }
        root->isRoot = true;
    }
    
    string getRoot(string word) {        
        Node* root = head;
        string res = "";
        for (char c: word) {
            if (root->next[c-'a'] == NULL) return word;
            res += c;
            root = root->next[c-'a'];
            if (root->isRoot) return res;
        }
        return word;
    }
    
    
    string replaceWords(vector<string>& dict, string sentence) {
        head = new Node();
        string res = "";
        for (string word: dict) {
            insert(word);
        }
        string currWord = "";
        for (char c: sentence) {
            if (c == ' ') {
                res += getRoot(currWord) + " ";
                currWord = "";
            } else {
                currWord += c;
            }
        }
        if (currWord != "") res += getRoot(currWord);
        return res;
    }
};
```