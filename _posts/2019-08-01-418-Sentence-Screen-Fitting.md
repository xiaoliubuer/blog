---
layout: post
title:  "418. Sentence Screen Fitting"
date: 2019-08-01 21:07:00 -0400
categories: articles
---
Given a rows x cols screen and a sentence represented by a list of non-empty words, find how many times the given sentence can be fitted on the screen.

Note:
```
A word cannot be split into two lines.
The order of words in the sentence must remain unchanged.
Two consecutive words in a line must be separated by a single space.
Total words in the sentence won't exceed 100.
Length of each word is greater than 0 and won't exceed 10.
1 ≤ rows, cols ≤ 20,000.
```
Example 1:
```
Input:
rows = 2, cols = 8, sentence = ["hello", "world"]

Output: 
1

Explanation:
hello---
world---

The character '-' signifies an empty space on the screen.
```
Example 2:
```
Input:
rows = 3, cols = 6, sentence = ["a", "bcd", "e"]

Output: 
2

Explanation:
a-bcd- 
e-a---
bcd-e-

The character '-' signifies an empty space on the screen.
```
Example 3:
```
Input:
rows = 4, cols = 5, sentence = ["I", "had", "apple", "pie"]

Output: 
1

Explanation:
I-had
apple
pie-I
had--

The character '-' signifies an empty space on the screen.
```
```c++
class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        string s = "";
        for ( string &x : sentence ) s += x+" ";
        
        int n = s.size();
        vector<int> offset(n);
        
        for ( int i=0, j=0; i<n; i++ ) {
            if ( s[i]==' ' ) j=0;
            else j++;
            offset[i] = j;
        }
        
        int k = 0;
        for ( int i=0; i<rows; i++ ) {
            k += cols;
            k = k - offset[k%n] + 1;
        }
        return k/n;
    }
};
```
```c++
class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
        string s = ""; // new string of sentence
        for(int i=0; i<sentence.size(); i++){
        	s += sentence[i];
        	s += " ";
		}
		int n = s.length();
		int cnt = 0; // input's length already in the screen
		for(int i=0; i<rows; i++){
			cnt += cols;
			if(s[cnt%n] == ' '){
				cnt++;
			}
			else{
				while(s[(cnt-1)%n] != ' ' && cnt>0){
					cnt--;
				}
			}
		}
		return cnt/n;
    }
    
};
```
```c++
class Solution {
public:
    int wordsTyping(vector<string>& sentence, int rows, int cols) {
    	unordered_map<int, int> umap; //创建一个map来记录什么呢？？？
    	int num = 0, n = sentence.size();
    	for (int i = 0; i < rows; ++i){
    		//要很好处理记录在换行的时候，这个下标走到那个单词下面了.在新的一样从那个的下一个单词开始进行添加
    		int start = num % n;
    		if(umap.count(start) == 0){
    		int cnt = 0;
    		int len = 0;
    		for (int j = start; len < cols; j = (j+1)%n, cnt++ ){ //这个地方非常重要 j = (j+1) % n
    			if ( len + sentence[j].size() > cols ) break;
    			len += sentence[j].size() + 1;
    		}
    		num += cnt;
    		umap.emplace(start, cnt);
    		}
    		else
    			num += umap[start];
		}
    	return num / n; //以及最后算这个答案的方式，用累加起来的长度去除以sentence的长度.
    }
};
```