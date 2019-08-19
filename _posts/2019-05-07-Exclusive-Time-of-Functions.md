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
    vector<int> exclusiveTime(int n, vector<string>& logs)    {
        vector<int> vec(n, 0);
        //std::cout << n << std::endl;
        std::stack<int> st;
        int prev_val = 0;
        for(int i = 0; i < logs.size(); ++i) {
            int pos_first_colon = logs[i].find(":");
            int pos_second_colon = logs[i].find(":", pos_first_colon + 1);
            
            int idx = stoi( logs[i].substr(0, pos_first_colon) );
            std::string entry = logs[i].substr(pos_first_colon + 1, pos_second_colon - pos_first_colon -1);
            int curr_val = stoi( logs[i].substr(pos_second_colon + 1, logs[i].length() - pos_second_colon - 1) );
            
            if(not st.empty()) {
                vec[st.top()] += curr_val - prev_val;
            }
            prev_val = curr_val;
            if(entry == "start") {
                st.push(idx);
            }
            else {
                int idx = st.top();
                ++vec[idx];
                st.pop();
                ++prev_val;
            }
        }
        return vec;
    }
};
```
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
```c++
class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        stack<int> funcInfo;
        int logSize=logs.size();
        vector<int> ans(n,0);
        stack<pair<int,int>> S; 
        int depth=0;
        for(int i=0;i<logSize;i++){
            int start=0;
            int found=logs[i].find_first_of(":");
            int id=atoi(logs[i].substr(start,found-start).c_str());
            start=found+1;
            found=logs[i].find_first_of(":",start);
            string state=logs[i].substr(start,found-start);
            long long timestamp=atoll(logs[i].substr(found+1).c_str());
            if(state=="start"){
                S.push(make_pair(id, timestamp));
            }else{
                int duration=timestamp;
                while(S.top().first==-1){
                    duration-=S.top().second;
                    S.pop();
                }
                int startTimestamp=S.top().second;
                duration-=startTimestamp;
                ans[S.top().first]+=(duration+1);
                S.pop();
                S.push(make_pair(-1, timestamp-startTimestamp+1));
            }
        }
        return ans;
    }
};
```
```c++
class Solution {
public:
    //Time complexity: O((#chars in logs) + logs.size()*log(logs.size()) + n)
    //Space complexity: O(n);
    vector<int> exclusiveTime(int n, vector<string>& logs){
        vector<int> ans(n, 0);
        if(logs.empty()){
            return ans;
        }
        vector<Triple> states; //id, time, state(start = 1, end = 0)
        for(const string &log : logs){
            int pos1 = log.find(':');
            int pos2 = log.find(':', 1 + pos1);
            int id = stoi(log.substr(0, pos1));
            int time = stoi(log.substr(1 + pos2, log.size() - 1 - pos2));
            int state = log[pos1+1] == 's' ? 1 : 0;
            states.push_back(Triple(id, time, state));
        }
        
        sort(states.begin(), states.end(),
            [](const Triple &t1, const Triple &t2){
                if(t1.time != t2.time){
                    return t1.time < t2.time;
                }
                if(t1.id != t2.id){
                    return t1.state < t2.state; //For different tasks, first end, and then start
                }
                return t1.state > t2.state;//For the same task, first start, and then end
            }
        );
        
        int num = states.size();
        stack<pair<int, int>> function_stk; //id, time//store the starting time of the functions
        for(int i = 0; i < num; ++i){
            if(!function_stk.empty()){
                ans[function_stk.top().first] += states[i].time - function_stk.top().second + 1 - states[i].state;
            }
            if(states[i].state == 1){
                function_stk.push({states[i].id, states[i].time});
            }
            else{
                function_stk.pop();
                if(!function_stk.empty()){
                    function_stk.top().second = 1 + states[i].time;
                }
            }
            //cout << states[i].id << ' ' << states[i].time << ' ' << states[i].state <<endl;
        }
        
        return ans;
    }
    
private:
    struct Triple{
        int id;
        int time;
        int state;
        Triple(int t_id, int t_time, int t_state){
            id = t_id;
            time = t_time;
            state = t_state;
        }
    };
};
```