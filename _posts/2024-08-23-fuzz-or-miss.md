---
layout: post
title:  "Fuzz or Miss: Uncovering Hidden Bugs"
author: chandler
categories: [ pentests, bug bounty]
image: assets/images/2.jpg
featured: true
---

Earlier this year, I was part of a team conducting a penetration test for a client's publicly accessible web application. Although I won't specify the site or CMS used, the experience highlighted a vital aspect of security testing.

Over two weeks, we meticulously followed the OWASP testing guide, extensively enumerating every feature of the application. Despite our thorough efforts, we initially found no vulnerabilities. It wasn't until the end of our assignment that I decided to delve deeper by fuzzing directories several levels deep, driven by a determination to find anything that might have been overlooked.

Utilizing various wordlists, I began probing deeper into the application’s directory structure. Interestingly, I discovered that accessing /01 redirected to a group's main page, while /02 led to a secondary page, if it existed. Encouraged by this pattern, I continued with /03, /04, and so on. This approach started revealing new, unindexed pages, some of which were admin pages that had not been identified in earlier phases of testing.

To exploit this finding systematically, I created a wordlist ranging from 1 to 50 and resumed fuzzing across all known directory groups. This unearthed several intriguing pages, but one really stood out: it was used by admins to send notifications to various user groups within the application, including other Admins, regular users, and other specific groups.

To assess the security of this notification feature, I executed a test by sending a notification to our pentester group that included an XSS payload:
```bash
'"><img src=1 onerror=console.log(document.domain)
```

Unexpectedly, when we received the notifications, the page rendered our tags and executed the JavaScript, confirming a vulnerability. This experience underscores the indispensable value of fuzzing in penetration testing and bug bounty hunting. As pentesters, we don't have all the time that threat actors do to enumerate our target. And so, we must prioritze our activities. If you don't fuzz, you truly don't know what vulnerabilities lie hidden, waiting to be exploited. This single finding alone illustrates why thorough testing, beyond standard enumeration, is crucial for uncovering deeper security flaws. Try harder!
