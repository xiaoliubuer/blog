---
layout: post
title:  "535. Encode and Decode TinyURL"
date: 2019-08-07 21:29:00 -0400
categories: articles
---
Note: This is a companion problem to the System Design problem: Design TinyURL.
TinyURL is a URL shortening service where you enter a URL such as https://leetcode.com/problems/design-tinyurl and it returns a short URL such as http://tinyurl.com/4e9iAk.

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

```c++
class Solution {
public:

    string getRand(){
        srand(time(NULL));
        string sb = "";
        for (int i = 0; i < 6; ++i){
            sb += chars[rand()%62];
        }
        return sb;
    }

    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        while(short_to_long[key] != "")
            key = getRand();
        short_to_long[key] = longUrl;
        return "https://tinyurl.com/" + key;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return short_to_long[shortUrl.substr(shortUrl.find(".com/") + 5)];
    }
private:
    string chars = "0123456789qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM";
    string key = getRand();
    unordered_map<string, string> short_to_long;
};
```