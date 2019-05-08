---
layout: post
title:  "636. Exclusive Time of Functions"
date: 2019-05-07 08:11:00 -0400
categories: articles
---
On a single threaded CPU, we execute some functions.  Each function has a unique id between 0 and N-1.

We store logs in timestamp order that describe when a function is entered or exited.

Each log is a string with this format: "{function_id}:{"start" | "end"}:{timestamp}".  For example, "0:start:3" means the function with id 0 started at the beginning of timestamp 3.  "1:end:2" means the function with id 1 ended at the end of timestamp 2.

A function's exclusive time is the number of units of time spent in this function.  Note that this does not include any recursive calls to child functions.

Return the exclusive time of each function, sorted by their function id.

```c++
class Solution {
public:
    struct MyLog{
        int id;
        string status;
        int time;
    };
    
    vector<string> split(string target, char delimiter){ // split string to vector
        vector<string> tokens;
        string token;
        istringstream iss(target);
        while( getline(iss, token, delimiter)){
            tokens.push_back(token);
        }
        return tokens;
    }
    
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        stack<MyLog> buffer;
        vector<MyLog> newlogs;
        vector<int> res(n, 0);
        for ( auto i : logs) {
            vector<string> output = split(i, ':');
            MyLog log;
            log.id = stoi(output[0]);
            log.status = output[1];
            log.time = stoi(output[2]);
            newlogs.push_back(log);
        }
        buffer.push(newlogs[0]);
        int pre = newlogs[0].time;
        for (int i = 1; i < newlogs.size(); i++) {
            if ( newlogs[i].status == "start"){
                if (!buffer.empty())
                    res[buffer.top().id] += newlogs[i].time - pre;
                buffer.push(newlogs[i]);
                pre = newlogs[i].time;
            }
            else{
                res[buffer.top().id] += newlogs[i].time - pre + 1;
                buffer.pop();
                pre = newlogs[i].time + 1;
            }
        }
        return res;
    }
};
```

```c++
// Shame Answer
class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        vector<int> ret(n, 0);
        if (logs.size() < 2) return ret;
        stack<int> function_ids;
        int pre_ts = 0;
        for (int i = 0; i < logs.size(); ++i) {
            int id = stoi(logs[i].substr(0, logs[i].find(':')));
            int ts = stoi(logs[i].substr(logs[i].find_last_of(':') + 1));
            bool is_start = logs[i].find("start") != string::npos;
            if (!function_ids.empty()) {
                ret[function_ids.top()] += ts - pre_ts;
            }
            if (is_start) {
                function_ids.push(id);
                pre_ts = ts;
            }
            else {
                ret[function_ids.top()] += 1;
                function_ids.pop();
                pre_ts = ts + 1;
            }
        }
        return ret;
    }
};
```