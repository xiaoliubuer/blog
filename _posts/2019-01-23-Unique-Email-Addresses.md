---
layout: post
title:  "929. Unique Email Addresses"
date: 2019-01-23 20:32:23 -0400
categories: articles
---
Every email consists of a local name and a domain name, separated by the @ sign.

For example, in alice@leetcode.com, alice is the local name, and leetcode.com is the domain name.

Besides lowercase letters, these emails may contain '.'s or '+'s.

If you add periods ('.') between some characters in the local name part of an email address, mail sent there will be forwarded to the same address without dots in the local name.  For example, "alice.z@leetcode.com" and "alicez@leetcode.com" forward to the same email address.  (Note that this rule does not apply for domain names.)

If you add a plus ('+') in the local name, everything after the first plus sign will be ignored. This allows certain emails to be filtered, for example m.y+name@email.com will be forwarded to my@email.com.  (Again, this rule does not apply for domain names.)

It is possible to use both of these rules at the same time.

Given a list of emails, we send one email to each address in the list.  How many different addresses actually receive mails? 

 

Example 1:
```
Input: ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]
Output: 2
Explanation: "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails
```

Note:

1 <= emails[i].length <= 100
1 <= emails.length <= 100
Each emails[i] contains exactly one '@' character.
# 题意
就是对一个字符串进行一堆处理，然后返回处理后的样子。
# 想法
用STL里面的函数对其进行处理。

# Shame answer
```c++
class Solution
{
public:

    using Emails = vector< string >;
	using Unique = unordered_set< string >;

    int numUniqueEmails( Emails& emails, Unique unique={} )
	{
        for( auto& e: emails )
		{
            auto pivot = e.find_first_of('@'); auto name = e.substr( 0, pivot ), domain = e.substr( pivot );
            name.erase( remove( name.begin(), name.end(), '.' ), name.end() );
            unique.insert( name.erase( name.find_first_of('+') ) + domain );
        }
        return static_cast< int >( unique.size() );
    }
	
};
```