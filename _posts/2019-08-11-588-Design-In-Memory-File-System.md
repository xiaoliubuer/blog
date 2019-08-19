---
layout: post
title:  "588. Design In-Memory File System"
date: 2019-08-11 17:54:00 -0400
categories: articles
---	
Design an in-memory file system to simulate the following functions:

ls: Given a path in string format. If it is a file path, return a list that only contains this file's name. If it is a directory path, return the list of file and directory names in this directory. Your output (file and directory names together) should in lexicographic order.

mkdir: Given a directory path that does not exist, you should make a new directory according to the path. If the middle directories in the path don't exist either, you should create them as well. This function has void return type.

addContentToFile: Given a file path and file content in string format. If the file doesn't exist, you need to create that file containing given content. If the file already exists, you need to append given content to original content. This function has void return type.

readContentFromFile: Given a file path, return its content in string format.
Example:
```
Input: 
["FileSystem","ls","mkdir","addContentToFile","ls","readContentFromFile"]
[[],["/"],["/a/b/c"],["/a/b/c/d","hello"],["/"],["/a/b/c/d"]]

Output:
[null,[],null,null,["a"],"hello"]
```
Explanation:
```
filesystem
```
Note:
```
You can assume all file or directory paths are absolute paths which begin with / and do not end with / except that the path is just "/".
You can assume that all operations will be passed valid parameters and users will not attempt to retrieve file content or list a directory or file that does not exist.
You can assume that all directory names and file names only contain lower-case letters, and same names won't exist in the same directory.
```
```c++
struct FS {
    string key;
    string value;
    map<string, FS*> paths;
    FS(string input) {
        key = input;
        value = "";
    }
};

class FileSystem {
public:
    
    vector<string> stringSplit(string path) {
        stringstream ss(path);
        vector<string> p;
        int count = 0;
        
        // When we are splitting the string by /, we dont want the first empty string, hence the count
        // variable is used.
        while(getline(ss, path, '/')) {
            if (count > 0) p.push_back(path);
            count++;
        }
        
        return p;
    }

    FS *root;
    FileSystem() {
        root = new FS("");
    }
    
    FS* advanceNode(string path) {
        FS* temp = root;
        for (auto p : stringSplit(path)) temp = temp->paths[p];
        return temp;
    }
    
    FS* createAndAdvanceNode(string path) {
        FS* temp = root;
        for (auto p : stringSplit(path)) {
            if (temp->paths.find(p) == temp->paths.end())
                temp->paths[p] = new FS(p);
            temp = temp->paths[p];
        }
        return temp;
    }
    
    vector<string> ls(string path) {
        vector<string> ans;
        FS* temp = advanceNode(path);
        for (auto it = temp->paths.begin(); it != temp->paths.end(); ++it)
            ans.push_back(it->first);
        // If this is a file its value will be non-empty
        // Hence we need to return this in the call to ls as well.
        if (temp->value != "") ans.push_back(stringSplit(path).back());
        return ans;
    }
    
    void mkdir(string path) {
        createAndAdvanceNode(path);
    }
    
    void addContentToFile(string filePath, string content) {  
        FS* temp = createAndAdvanceNode(filePath);
        temp->value = temp->value + content;
    }
    
    string readContentFromFile(string filePath) {
        return advanceNode(filePath)->value;
    }
};
```
```c++
class FileSystem {
public:
    
    struct file_sys {
        set<string> all;
        unordered_map<string, string> file;
        unordered_map<string, file_sys*> dir;
    };
    
    file_sys *root;
    
    FileSystem() {
        root = new file_sys;
    }
    
    vector<string> ls(string path) {
        string cur_dir = "";
        if (path.back() != '/') path += '/';
        file_sys *p = root;
        vector<string> res;
        for (int i = 1; i < path.length(); i++) {
            if (path[i] == '/') {
                if (p->file.find(cur_dir)!=p->file.end()) {
                    if (i == (path.length()-1)) {
                        return {cur_dir};
                    } else {
                        return {};
                    }
                }
                if (p->dir.find(cur_dir)!=p->dir.end()) {
                    p = p->dir[cur_dir];
                } else {
                    return {};
                }
                cur_dir = "";
            } else {
                cur_dir += path[i];
            }
        }
        for (auto it = p->all.begin(); it != p->all.end(); ++it) {
            res.push_back(*it);
        }
        return res;
    }
    
    void mkdir(string path) {
        string cur_dir = "";
        if (path.back() != '/') path += '/';
        file_sys *p = root;
        for (int i = 1; i < path.length(); i++) {
            if (path[i] == '/') {
                if (p->dir.find(cur_dir)!=p->dir.end()) {
                    p = p->dir[cur_dir];
                } else {
                    file_sys *q = new file_sys;
                    p->dir[cur_dir] = q;
                    p->all.insert(cur_dir);
                    p = q;
                }
                cur_dir = "";
            } else {
                cur_dir += path[i];
            }
        }
    }
    
    void addContentToFile(string filePath, string content) {
        string path = filePath.substr(0, filePath.rfind('/')+1);
        string file_name = filePath.substr(filePath.rfind('/')+1);
        string cur_dir = "";
        file_sys *p = root;
        for (int i = 1; i < path.length(); i++) {
            if (path[i] == '/') {
                if (p->dir.find(cur_dir)!=p->dir.end()) {
                    p = p->dir[cur_dir];
                } else {
                    return;
                }
                cur_dir = "";
            } else {
                cur_dir += path[i];
            }
        }
        if (p->file.find(file_name) == p->file.end()) {
            p->file[file_name] = content;
            p->all.insert(file_name);
        } else {
            p->file[file_name] += content;
        }
    }
    
    string readContentFromFile(string filePath) {
        string path = filePath.substr(0, filePath.rfind('/')+1);
        string file_name = filePath.substr(filePath.rfind('/')+1);
        string cur_dir = "";
        file_sys *p = root;
        for (int i = 1; i < path.length(); i++) {
            if (path[i] == '/') {
                if (p->dir.find(cur_dir)!=p->dir.end()) {
                    p = p->dir[cur_dir];
                } else {
                    return "";
                }
                cur_dir = "";
            } else {
                cur_dir += path[i];
            }
        }
        if (p->file.find(file_name) == p->file.end()) {
            return "";
        } else {
            return p->file[file_name];
        }
    }
};
```
```c++
class FileSystem {
public:
    FileSystem() {
        dirs["/"];
    }
    
    vector<string> ls(string path) {
        if(files.count(path)){
            return {path.substr(path.find_last_of("/")+1)};
        }
        return vector<string> (dirs[path].begin(), dirs[path].end());
    }
    
    void mkdir(string path) {
        istringstream is(path);
        string t, dir;
        while(getline(is, t, '/')){
            if(t.empty()) continue;
            if(dir.empty()) dir+="/";
            dirs[dir].insert(t);
            if(dir.size() > 1) dir+="/";
            dir+=t;
        }
    }
    
    void addContentToFile(string filePath, string content) {
        int pos = filePath.find_last_of("/");
        auto dir = filePath.substr(0, pos);
        auto file = filePath.substr(pos+1);
        if(dir=="") dir="/";
        if(!dirs.count(dir)) mkdir(dir);
        dirs[dir].insert(file);
        files[filePath].append(content);
    }
    
    string readContentFromFile(string filePath) {
        return files[filePath];
    }
    unordered_map<string, set<string>> dirs;
    unordered_map<string, string> files;
};
```
```c++
class Node{
public:
	string name, content;
	bool is_file;
	map<string, Node*> children;
	
	Node(string name, bool is_file){
		this->name=name;
		this->is_file=is_file;
	}
};

class FileSystem {
private:
	Node* head;
	
	vector<string> parse(string dir){
		vector<string> res;
		string cur;
		for(int i=1;i<dir.size();++i){
			if(dir[i]!='/') cur.push_back(dir[i]);
			else{
				res.push_back(cur);
				cur="";
			}
		}
		if(cur.size()>0) res.push_back(cur);
		return res;
	}

public:
    FileSystem() {
		head=new Node("/", false);
    }
	
    vector<string> ls(string path) {
        vector<string> p=parse(path), res;
		Node* cur=head;
		for(int i=0;i<p.size();++i) cur=(cur->children)[p[i]];
		if(cur->is_file) res.push_back(cur->name);
		for(auto& t:(cur->children)) res.push_back(t.first);
		return res;
    }
    
    void mkdir(string path) {
		vector<string> p=parse(path);
        Node* cur=head;
		for(int i=0;i<p.size();++i){
			if((cur->children).find(p[i])==(cur->children).end()) (cur->children)[p[i]]=new Node(p[i],false);
			cur=(cur->children)[p[i]];
		}
		return;
    }
    
    void addContentToFile(string filePath, string content) {
		mkdir(filePath);
        vector<string> p=parse(filePath);
		Node* cur=head;
		for(int i=0;i<p.size();++i) cur=(cur->children)[p[i]];
		cur->is_file=true;
		cur->content=(cur->content)+content;
		return;
    }
    
    string readContentFromFile(string filePath) {
        vector<string> p=parse(filePath);
		Node* cur=head;
		for(int i=0;i<p.size();++i) cur=(cur->children)[p[i]];
		return cur->content;
    }
};
```