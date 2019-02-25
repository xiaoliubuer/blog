---
layout: post
title:  "Amazon Hire Event"
date: 2019-02-24 14:41:23 -0400
categories: articles
---

***
02/24/2019


一共四轮，中间不休息，连续4小时，强度有点大啊，面完第三轮觉得脑子被榨干了 T^T

第一轮，自我介绍一下，问了project，把整个application框架画出来，具体说明每部分干什么，用什么database，为什么要选择那种database，因为我们用的是azure，问了很多为什么要用azure blob之类的问题。__LRU Cache__，高频的不能再高频了。我给了hashmap+doubleLinkedList的解法，写完后又问除了doubleLinkedList还可以用其他数据结构吗。举例说明Tree可不可以。我口头分析了一下觉得get比较容易完成，但是set比较难因为需要重新balance整个树。最后两个bq结束。


第二轮，题目: 说有一个记录每个人点击一些页面的顺序的log，比如用户a点击了page1，4，5，2，7，8.。。。，用户b点击了5，3，7，8，2，3.。。。总之不确定个数的用户，log记录他们点击页面的顺序，要求统计点击频率最高的连续三个页面组成的sequence。比如a点击过的sequence有（1，4，5）， （4，5，2）， （5，2，7）。。。需要统计所有人的。我给出了一个用hashmap和MaxHeap的解法。然后他问如果数据很大，或者中间统计了很多没有用的数据，怎么优化，除了heap还有别的方法吗。说了半天我感觉他在往map reduce上引导，最后我就说了map reduce，然后画了个图，大概说明map部分和reduce部分分别做什么。不知道他最后满意不满意。。。


第三轮，一个印度人带一个白人小哥shadow。先问了些bq，然后开始做题。第一道是LCA变种，加入了parent pointer，我给了两种解法，一种是用left right， 一种是用parent，用parent的一开始我口头说了个比较慢的方法，然后写了一个O（logn）的方法。第二道题是给一个linkedlist，要求每两个node为一对，swap每一对，最后如果单独留下一个node就不变。写的时候感觉好累，连续几轮不停说话脑子已经一团浆糊，所以里面有点小bug，在口头debug的时候自己找到改过来了，最后应该是写对了。写完后还有点时间又问了俩bq（我恨bq T^T）


第四轮，印度女hiring manager，应该就是bar raiser。给人感觉非常nice，问我感觉咋样我说好累（555），然后她说她说这个大组的manager，负责这次招人，这轮跟我聊一些问题希望能了解我的personality，然后我们一起来设计一个系统吧～反正让人觉得蛮友好的。这轮的bq是1. 如果有很多事要同时处理，如何prioritize，2. 说一个没有赶上deadline的例子，3.如何提高工作中的效率，4.是否在工作中收到过不好的feedback，然后系统设计：TinyURL，由于准备过，就还比较顺利写完了，然后又一起做了些优化，比如按照国家存cache，sharding什么的。最后面完她把我送出来了，路上闲聊西雅图糟糕的冬天。。。

总体来说体验还不错，住的不错，整个面试过程没有太多等待，每组的面试官都很nice，就是。。。这组。。。感觉60%以上的印度人了吧？

bq问题我是记不住顺序，但是大概考了这些：
有没有过付出很多努力才解决的问题，有没有犯过什么错误，有没有过跟老板意见不合但你坚持了，在工作中的自我修养（自我提高，比如学习新技术啥的），赶不上deadline怎么办，在可以停下的时候但你选择了继续，你有没有create a innovative product。总之那个经典bq帖子上的问题问到了70%我的天。。。

***
02/23/2019 Upload 02/24/2019 View<br>
第一面，bar raiser 一堆BQ，然后设计题，设计一个给客户发通知邮件的系统<br>
第二面，BQ，合并k个列表，Leetcode 23 [link](http://localhost:4000/articles/2019/01/02/Merge-k-Sorted-Lists.html)<br>
第三面，BQ， 编程题是把一堆路径，建成树状，有点像Trie那样的建树过程，用递归秒了。然后又问怎么做一个文件的搜索API，怎么设计各种条件，例如，文件名，文件大小，等等条件。我blabla说了一下设计模式<br>
第四面，BQ，介绍一下自己写的系统，画一下架构，讨论一下分布式系统的怎么扩展之类<br>
结果HR说老年给不了，可能是bar raiser那轮不太好。题目都很简单，建议大家多复习BQ，想想怎么吹。<br>

***
02/23/2019 upload 02/24/2019 View<br>
第一轮三姐manager，迟到了几分钟，bq：most challenging problem, tell me about an important decision you made, what was the result.<br/>
第二轮美国小哥，bq：tell me about a technique you recently learned, how do you get hands on it. 之后让我讲了讲具体的细节 coding：实现file search，自己定folder类和一个tree结构的目录，给一个path字符串输出这个path下的所有文件<br>
第三轮三哥，也是迟到了几分钟（真的爱迟到），bq：a time when you can't meet deadline, a time when you change the approach for customer , 然后问了具体如何change的，想没想过alter solution. coding：给一个包含a和b的字符串，找一个区间，将其中a和b互换，使字符串含b最多
