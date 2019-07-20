---
layout: post
title:  "Daily Records"
date: 2019-06-27 13:06:00 -0400
categories: articles
---
# [Reference](https://yzygitzh.github.io/algorithm/2018/11/09/leetcode-summary.html)


---
07/20/2019


---
07/18/2019
* [1079. Letter Tile Possibilities](http://localhost:4000/articles/2019/07/18/Letter-Tile-Possibilities.html) (M) !!!!!
	* M: The total combination of a string of characters
	* S: 
	* TC:  SC:
* M [320. Generalized Abbreviation](http://localhost:4000/articles/2019/07/18/Letter-Tile-Possibilities.html)
	* M: Write a function to generate the generalized abbreviations of a word.
	* S: 
	* TC:  SC:

* H [425. Word Squares](http://localhost:4000/articles/2019/07/18/Word-Squares.html)

---
07/16/2019
* [290. Word Pattern](http://localhost:4000/articles/2019/07/16/Word-Pattern.html) (E)
	* M: 
	* S: 
	* TC:  SC:
* [291. Word Pattern II](http://localhost:4000/articles/2019/07/16/Word-Pattern-II.html) (H)

---
07/15/2019
* [266. Palindrome Permutation](http://localhost:4000/articles/2019/03/26/Palindrome-Permutation.html)
	* M: Given a string, determine if a permutation of the string could form a palindrome.
	* S: 1. use set, to verify final set size is <= 1. 2. bitset
	* TC: SC:


* [267. Palindrome Permutation II](http://localhost:4000/articles/2019/07/16/Palindrome-Permutation-II.html)
	* M: Given a string s, return all the palindromic permutations (without duplicates) of it. Return an empty list if no palindromic permutation could be form.
	* S: Backtracking, find mid first, return number of mid > 1, get all first half permutation, genreate result with first_half + mid + first_half.
	* TC: SC:


* [31. Next Permutation](http://localhost:4000/articles/2019/03/23/Next-Permutation.html)
	* M: Get next permutation of a number array
	* S: Get index which nums[index - 1] < nums[index], put min after index to index-1, reverse array fron index to end
	* TC: O(n) SC: O(1)

* [254. Factor Combinations](http://localhost:4000/articles/2019/07/16/Factor-Combinations.html)
	* M: Numbers can be regarded as product of its factors
	* S: use divide and module to caculate, push to temp as pair
	* TC: SC:


---
07/11/2019
* [127. Word Ladder](http://localhost:4000/articles/2019/04/02/Word-Ladder.html)
	* M: min steps transform from beginword to endword based on dict
	* S: queue, unordered_set, bfs
	* TC: SC:

* [126. Word Ladder II](http://localhost:4000/articles/2019/07/13/Word-Ladder-II.html)
	* M: return all shortest path
	* S: Can use BST, but it is for path rather than word. Little be trickey
	* TC: SC:

* [433. Minimum Genetic Mutation](http://localhost:4000/articles/2019/04/02/Minimum-Genetic-Mutation.html)
	* M: same as 127
	* S: Same solution
	* TC: SC:

---
07/10/2019
* [93. Restore IP Addresses](http://localhost:4000/articles/2019/06/19/Restore-IP-Addresses.html)
	* M: give a string of number, to split them into a IP addresses, return all situations
	* S: split one by one, check each valid or not
	* TC: SC:
* [51. N-Queens](http://localhost:4000/articles/2019/07/10/N-Queens.html)
	* M: place N queens on N * N board, have to check row, col and diagonal
	* S: Backtracking, init vector<string> tmp as '........', '........', ..., change . to Q, check valid or not with col and diagonal, push valid row == n to result.
	* TC: SC: 

* [22. Generate Parentheses](http://localhost:4000/articles/2019/03/29/Generate-Parentheses.html)
	* M: Input n, return n "()" combination 
	* S: Backtracking, record number of '(' and number of ')'to add new '(' or ')' to the tmp string
	* TC:   SC: 

---
06/30/2019
* [784. Letter Case Permutation](http://localhost:4000/articles/2019/06/30/Letter-Case-Permutation.html)
	* M: Given a string with alpha and digit characters, return list of string with combination of all alpha charachter's lower and upper cases.
	* S: Bracking. each idx with two situations 'a' and 'A'. 
	* TC:  SC:

* [39. Combination Sum](http://localhost:4000/articles/2019/01/04/Combination-Sum.html)
	* M: All combination of given array equal to target
	* S: Backtracking. helper function put target - candicates[i], tmp. Meet require tmp arrays will be added to res.
	* TC:   SC: 

---
06/29/2019
* [130. Surrounded Regions](http://localhost:4000/articles/2019/04/02/Surrounded-Regions.html)
	* M: Change all surrounded 'O' to 'X'. The ones can reach to the edages stay.
	* S: DFS mark all t, l, d, r 4 edges to mark the points which connected to the edges. Then two embeded loops to change 'O' to 'X', 'M' to 'O'. 
	* TC: O(N), SC: O(1)

---
06/27/2019
* [129. Sum Root to Leaf Numbers](http://localhost:4000/articles/2019/06/25/Sum-Root-to-Leaf-Numbers.html)
	* M: From root to leaf as a number, sum all numbers 
	* S: Recursion, each level will be sum * 10 + root->val 
	* TC: O(N) SC: O(1)

---
06/26/2019

