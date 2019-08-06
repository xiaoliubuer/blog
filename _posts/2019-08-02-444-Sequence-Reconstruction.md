---
layout: post
title:  "444. Sequence Reconstruction"
date: 2019-08-02 20:35:00 -0400
categories: articles
---
Check whether the original sequence org can be uniquely reconstructed from the sequences in seqs. The org sequence is a permutation of the integers from 1 to n, with 1 ≤ n ≤ 104. Reconstruction means building a shortest common supersequence of the sequences in seqs (i.e., a shortest sequence so that all sequences in seqs are subsequences of it). Determine whether there is only one sequence that can be reconstructed from seqs and it is the org sequence.

Example 1:
```
Input:
org: [1,2,3], seqs: [[1,2],[1,3]]

Output:
false
```
Explanation:
[1,2,3] is not the only one sequence that can be reconstructed, because [1,3,2] is also a valid sequence that can be reconstructed.
```
Example 2:
```
Input:
org: [1,2,3], seqs: [[1,2]]

Output:
false
```
Explanation:
The reconstructed sequence can only be [1,2].
Example 3:
```
Input:
org: [1,2,3], seqs: [[1,2],[1,3],[2,3]]

Output:
true
```
Explanation:
The sequences [1,2], [1,3], and [2,3] can uniquely reconstruct the original sequence [1,2,3].
Example 4:
```
Input:
org: [4,1,5,2,6,3], seqs: [[5,2,6,3],[4,1,5,2]]

Output:
true
```
UPDATE (2017/1/8):
The seqs parameter had been changed to a list of list of strings (instead of a 2d array of strings). Please reload the code definition to get the latest changes.
```c++
class Solution {
public:
    bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
        if (seqs.empty()) return false;
        vector<int> pos(org.size()+1);
        for (int i = 0; i < org.size(); i++) pos[org[i]] = i;
        vector<char> flags(org.size()+1, 0);
        int tomatch = org.size()-1, cnt = 0;
        for (const auto& v : seqs)
            for (int i = 0; i < v.size(); i++, cnt++) {
                if (v[i] <= 0 || v[i] > org.size()) return false;
                if (i == 0) continue;
                int x = v[i-1], y = v[i];
                if (pos[x] >= pos[y]) return false;
                if (flags[x] == 0 && pos[x]+1 == pos[y]) {
                    flags[x] = 1;
                    tomatch--;
                }
            }
        return cnt >= org.size() ? tomatch == 0 : false;
    }
};
```
```c++
class Solution {
public:
	bool sequenceReconstruction(vector<int> org, vector<vector<int>> seqs) {
		int n = org.size();
		vector<int> ht(n + 1), ht1(n);
		for (int i = 0; i < n; ++i) ht[org[i]] = i;
		for (auto& v : seqs) {
			if (v.empty()) continue;
			if (v.back() < 1 || v.back() > n) return 0;
			if (v.back() == org.back()) ht1[n-1] = 1;
			for (int i = 0; i < (int)v.size() - 1; ++i)
				if (v[i] < 1 || v[i] > n) return 0;
				else if (ht[v[i + 1]] <= ht[v[i]]) return 0;
				//else if (ht[v[i]] == n-1) return 0;
				else if (v[i + 1] == org[ht[v[i]] + 1]) ht1[ht[v[i]]] = 1;
		}
		for (int i = 0; i < n; ++i) if (!ht1[i]) return 0;
		return 1;
	}
};
```
```c++
class Solution {
public:
    bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs) {
		int n=org.size();
		vector<int> idx(n), pred(n, -1);
		bool exist=0;
		for (int i=0; i < n; ++i) idx[org[i]-1] = i;
		for (auto& v : seqs) {
			if (v.empty()) continue;
			if (v.back() < 1 || v.back() >n) return 0;
			for (int i=0; i < (int)v.size()-1; ++i) {
				int i0=v[i]-1, i1=v[i+1]-1;
				if (i0 < 0 || i0 >= n || i1 < 0 || i1 >= n) return 0;
				exist |= v[i] == org[0];	
				if (idx[i0] >= idx[i1]) return 0;
				pred[idx[i1]] = max(pred[idx[i1]], idx[i0]);
			}
			exist |= v.back() == org[0];
		}
		for (int i=1; i < n; ++i) if (pred[i] != i-1) return 0;
		return exist;
    }
};
```
```c++
class Solution {
public:
  bool sequenceReconstruction(vector<int>& org, vector<vector<int>>& seqs)
  {
    int n = static_cast<int>(org.size());
    build_graph(n, seqs);
    for (const auto&[i, v] : m_graph)
      if (has_circle(i)) return false;  
    if (m_sorted.size() != n) return false;    
    return is_unique(m_sorted);
  }

private:
  void build_graph(int n, const vector<vector<int>>& seqs)
  {
    for(const auto& seq : seqs)
      for(int i : seq)
        m_graph[i] = {};
       
    for (const auto& seq : seqs)
      for (int i = 1, m = static_cast<int>(seq.size()); i < m; ++i)
        m_graph[seq[i - 1]].insert(seq[i]);

    m_sorted.reserve(n + 1);
  }

  bool has_circle(int i)
  {
    if (m_states[i] == 1) return true;
    else if (m_states[i] == 2) return false;

    m_states[i] = 1;
    for (int j : m_graph[i])
      if (has_circle(j)) return true;

    m_states[i] = 2;
    m_sorted.push_back(i);
    return false;
  }
  
  bool is_unique(const vector<int>& sorted)
  {
    for (int i = 0, n = static_cast<int>(sorted.size()); i < n - 1; ++i)
      if (!m_graph[m_sorted[i + 1]].count(m_sorted[i]))
        return false;
    return true;
  }

  vector<int> m_sorted;
  unordered_map<int,int> m_states;
  unordered_map<int, unordered_set<int>> m_graph;
};
```