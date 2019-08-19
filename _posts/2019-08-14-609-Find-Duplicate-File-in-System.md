---
layout: post
title:  "609. Find Duplicate File in System"
date: 2019-08-14 19:43:00 -0400
categories: articles
---	

Given a list of directory info including directory path, and all the files with contents in this directory, you need to find out all the groups of duplicate files in the file system in terms of their paths.

A group of duplicate files consists of at least two files that have exactly the same content.

A single directory info string in the input list has the following format:

"root/d1/d2/.../dm f1.txt(f1_content) f2.txt(f2_content) ... fn.txt(fn_content)"

It means there are n files (f1.txt, f2.txt ... fn.txt with content f1_content, f2_content ... fn_content, respectively) in directory root/d1/d2/.../dm. Note that n >= 1 and m >= 0. If m = 0, it means the directory is just the root directory.

The output is a list of group of duplicate file paths. For each group, it contains all the file paths of the files that have the same content. A file path is a string that has the following format:

"directory_path/file_name.txt"

Example 1:
```
Input:
["root/a 1.txt(abcd) 2.txt(efgh)", "root/c 3.txt(abcd)", "root/c/d 4.txt(efgh)", "root 4.txt(efgh)"]
Output:  
[["root/a/2.txt","root/c/d/4.txt","root/4.txt"],["root/a/1.txt","root/c/3.txt"]]
```
Note:
```
No order is required for the final output.
You may assume the directory name, file name and file content only has letters and digits, and the length of file content is in the range of [1,50].
The number of files given is in the range of [1,20000].
You may assume no files or directories share the same name in the same directory.
You may assume each given directory info represents a unique directory. Directory path and file info are separated by a single blank space.
```
Follow-up beyond contest:
```
Imagine you are given a real file system, how will you search files? DFS or BFS?
If the file content is very large (GB level), how will you modify your solution?
If you can only read the file by 1kb each time, how will you modify your solution?
What is the time complexity of your modified solution? What is the most time-consuming part and memory consuming part of it? How to optimize?
How to make sure the duplicated files you find are not false positive?
```
```c++
class Solution {
public:
    vector<vector<string>> findDuplicate(vector<string>& paths) {
        ios_base::sync_with_stdio(false);
        cin.tie(NULL);
        unordered_map<string, vector<string>> seen;
        for (const auto& s: paths) {
            int pos = s.find_first_of(' ');
            string dir = s.substr(0, pos);
            pos++;
            for (int i = pos; i < s.size(); i++) {
                if (s[i] == ' ') {
                    pos = i+1;
                } else if (s[i] == '(') {
                    string fname = dir + "/" + string(s.begin()+pos, s.begin()+i);
                    int e = s.find_first_of(')', i+1);
                    string snt(s.begin()+i, s.begin()+e+1);
                    seen[snt].emplace_back(fname);
                    i = e;
                }
                
            }
        }
        vector<vector<string>> res;
        for (auto& kv: seen) {
            if (kv.second.size() > 1) res.emplace_back(kv.second);
        }
        return res;
    }
};
```
```c++
class Solution {
public:
    vector<vector<string>> findDuplicate(vector<string>& paths) {
	vector<vector<string>> result;
	vector<vector<string>> ret;
	map = unordered_map<string, int>();

	for (const auto& p : paths) {
	    int index = 0;
	    int index2 = p.find(' ');
	    string dir = p.substr(index, index2 - index);
	    index = index2 + 1;
	    while (index < p.size()) {
		index2 = p.find('(', index);
		string fullpath = dir +"/" + p.substr(index, index2 - index);
		index = index2 + 1;
		index2 = p.find(')', index);
		string key = p.substr(index, index2 - index);
		if (map.count(key) == 0) {
		    map[key] = ret.size();
		    ret.push_back(vector<string>{fullpath});
		} else {
		    ret[map[key]].push_back(fullpath);
		}
		index = index2 + 2;
	    }
	}

	for (auto& r : ret) {
	    if (r.size() > 1) {
		result.push_back(vector<string>());
		swap(result.back(), r);
	    }
	}

	return result;
    }
private:
    unordered_map<string, int> map;
};
```
```c++
class Solution {
public:
    vector<vector<string>> findDuplicate(vector<string>& paths) {
        unordered_map<string, vector<string>> duplicates;
        
        for (const auto& path : paths) {
            stringstream ss(path);
            string root;
            getline(ss, root, ' ');
            
            string file;
            while (getline(ss, file, ' ')) {
                auto l = file.find('(');
                auto r = file.find(')');
                
                duplicates[file.substr(l, r - l)].push_back(root + "/" + file.substr(0, l));
            }
        }
        
        vector<vector<string>> ret;
        for (const auto& duplicate : duplicates) {
            if (duplicate.second.size() != 1) ret.push_back(duplicate.second);
        }
        
        return ret;
    }
};
```