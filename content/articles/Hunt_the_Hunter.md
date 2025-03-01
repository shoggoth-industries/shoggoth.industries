---
title: "Report: 2025-03-1 "
date: 2025-03-01
desciption: "Detect IP who's hunting zombies by sending them True C2 request"
tags: [CTI, virustotal, C2, shodan]
---

# Introduction

After about two months of telemetry, we observed certain recurring behavior on our honeypots. 

One of these behaviors involves receiving a request that doesn't resemble anything we typically see.

```txt
145.ll|'|'|SGFjS2VkX0Q0OTkwNjI3|'|'|WIN-JNAPIER0859|'|'|JNapier|'|'|19-02-01|'|'||'|'|Win 7 Professional SP1 x64|'|'|No|'|'|0.7d|'|'|..|'|'|AA==|'|'|112.inf|'|'|SGFjS2VkDQoxOTIuMTY4LjkyLjIyMjo1NTUyDQpEZXNrdG9wDQpjbGllbnRhLmV4ZQ0KRmFsc2UNCkZhbHNlDQpUcnVlDQpGYWxzZQ==12.act|'|'|AA==
```

Yes, this is indeed the request we received. There is no POST or GET method, just this long string of characters.

Before questioning where it comes from, we can first identify clear elements, such as separators represented by pipes. There are also strings that resemble Base64 (those ending with == ) .

If we translate one of the lines from Base64, it gives the following result.

![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/cyberchef_base64.png)


## Explanation

This resembles a sequence of instructions and indeed mimics a command and control (C2) server request. However, there's a problem: Windows 7 SP1 is very old and highly unlikely to be used.

After some research, we realized that this is actually a template message for a service related to vulnerability research entities. I'll explain.

## Malware Hunter & Co.

In this specific example, it's a service named [Malware Hunter](https://malware-hunter.shodan.io/), maintained by Shodan. This service mimics real C2 requests to scan the internet and listen for responses. If there's a positive response, it means a zombie is active at the queried address.

Currently, Malware Hunter (MH) seems to test only about a dozen C2s. They are not the only ones doing this, but they are one of the few that openly acknowledge and name themselves.

![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/shodan_MH.png)

The other compagny found in the log is BinaryEdge (BE)

![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/shodan_BE.png)

## Data Refining

By parsing the requests and eliminating duplicates, we narrowed down to three distinct requests:

```txt
Occurence: 43 (60 days)
145.ll|'|'|SGFjS2VkX0Q0OTkwNjI3|'|'|WIN-JNAPIER0859|'|'|JNapier|'|'|19-02-01|'|'||'|'|Win 7 Professional SP1 x64|'|'|No|'|'|0.7d|'|'|..|'|'|AA==|'|'|112.inf|'|'|SGFjS2VkDQoxOTIuMTY4LjkyLjIyMjo1NTUyDQpEZXNrdG9wDQpjbGllbnRhLmV4ZQ0KRmFsc2UNCkZhbHNlDQpUcnVlDQpGYWxzZQ==12.act|'|'|AA==

Occurence: 23 (60 days)
lv|'|'|VHJvamFuX0M0NkY2RTk=|'|'|MARK|'|'|user|'|'|2013-11-22|'|'||'|'|Win XP|'|'|No|'|'|0.6.4|'|'|..|'|'||'|'|[endof]

Occurence: 10 (60 days)
238\x00ll|'|'|SGFjS2VkX0Q3NUU2QUFB|'|'|WIN-QZN7FJ7D1O|'|'|Administrator|'|'|18-11-28|'|'||'|'|Win 7 Ultimate SP1 x64|'|'|No|'|'|S17|'|'|..|'|'|SW5ib3ggLSBPdXRsb29rIERhdGEgRmlsZSAtIE1pY3Jvc29mdCBPdXRsb29rAA==|'|'|
```

What's interesting is that both BinaryEdge (BE) and MalwareHunter (MH) seem to use the first two requests. However, we will soon see the third and most intriguing one.

Once we gathered the source IP addresses, we created a [collection](https://www.virustotal.com/gui/collection/b980f359f9ffe34da84d5839faac941bbb88ea2b9879291004ed946a5ea06d4c)  on VirusTotal, which revealed the following:

VT_collection
![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/VT_collection.png)

## Copycat

Okay, so they are all detected, which confirms that I'm not venturing into the unknown. 

However, something bothers me: it might be a coincidence, but what is this IP doing in the Netherlands when all the others are in the USA?

The answer could be quite simple: it might be someone trying to imitate BinaryEdge or Shodan. For the third listed request, there are only three associated IPs.

![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/3_IP.png)

![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/VT_collection_HL.png)

On this IP, there is no reference to an entity in the domain or hostname. 

And this scan doesn't fit into a unique logic of C2 scanning, unlike BE and MH.

![Decode base 64 with cyberchef.](/content/articles/Hunt_the_Hunter_picture/VT_comment.png)

So, we have proof here that a third entity, which hasn't revealed itself, uses the same techniques as C2 hunters but does not focus lonely on C2 hunting.

**Note:** This could also just be an IP serving as a shared proxy among multiple users, and thus the requests from it are decorrelated.


# Conclusions

We will continue to add to the virustotal collection when we receive new requests of this type.
