---
layout: post
title:  "71. Simplify Path"
date: 2019-03-08 20:03:23 -0400
categories: articles
---
Given an absolute path for a file (Unix-style), simplify it. Or in other words, convert it to the canonical path.

In a UNIX-style file system, a period . refers to the current directory. Furthermore, a double period .. moves the directory up a level. For more information, see: Absolute path vs relative path in Linux/Unix

Note that the returned canonical path must always begin with a slash /, and there must be only a single slash / between two directory names. The last directory name (if it exists) must not end with a trailing /. Also, the canonical path must be the shortest string representing the absolute path.

 

Example 1:
```
Input: "/home/"
Output: "/home"
Explanation: Note that there is no trailing slash after the last directory name.
```
Example 2:
```
Input: "/../"
Output: "/"
Explanation: Going one level up from the root directory is a no-op, as the root level is the highest level you can go.
```
Example 3:
```
Input: "/home//foo/"
Output: "/home/foo"
Explanation: In the canonical path, multiple consecutive slashes are replaced by a single one.
```
Example 4:
```
Input: "/a/./b/../../c/"
Output: "/c"
```
Example 5:
```
Input: "/a/../../b/../c//.//"
Output: "/c"
```
Example 6:
```
Input: "/a//b////c/d//././/.."
Output: "/a/b/c"
```

# 这样的题碰到了我基本就直接投降了！！！

# 参考答案
```c++
string simplifyPath(string path) {
	string ret;
	bool is_last_dir = false;
	int period_len = 0;
	for (int i = 0; i < path.length(); i++)
	{
		if (path[i] == '.') {
			bool tmp = isRelativePath(path, i, is_last_dir, period_len);
			if (tmp) {	/* is relative path */
				++i;
				if (is_last_dir) {
					del_eol(ret);
					ret.erase(ret.find_last_of('/') + 1);
					++i;
				}
			}
			else {
				ret += path.substr(i, period_len);
				i = i + period_len - 1;
			}
			continue;
		}
		else if (path[i] == '/') {
			if (i + 1 < path.length() && path[i + 1] == '/') {
				for (size_t j = i + 1; i < path.length(); j++){
					if (path[j] != '/') { i = j - 1; break;}
				}
			}
			
			if(ret.length() == 0 || ret[ret.length() - 1] != '/') ret += '/';
			continue;
		}
		ret += path[i];
	}
	del_eol(ret);
	return ret;
}

/* remove the '/' at the end of the string */
void del_eol(string & ret) {
	if (ret.length() > 1) {
		int i = ret.length() - 1;
		for (; i > 0; --i)
			if (ret[i] != '/')
				break;
		ret.erase(i + 1);
	}
}

/* 
	judge if the substring is relative path 
	note: 'is_last_dir' represents the parent directory. it can be used , only if the method return true
*/
bool isRelativePath(string &path, int &i, bool &is_last_dir, int &period_len) {
	period_len = 0;  //the length of period
	is_last_dir = false;
	for (int j = i; j < path.length(); j++, period_len++)
	{
		if (path[j] != '.') {
			if (path[j] == '/' && period_len <= 2) {
				is_last_dir = period_len == 2 ? true : false;
				return true;
			}
			return false;
		}
	}
	
	is_last_dir = period_len == 2 ? true : false;
	return period_len <= 2;
}
```