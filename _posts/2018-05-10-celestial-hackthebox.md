---
layout: post
title: Celestial - Hackthebox
date: 2018-05-10 20:29
author: rowbot
comments: true
categories: [Hackthebox]
---
nmap scan of box
<pre>Nmap scan report for 10.10.10.85
Host is up, received reset ttl 63 (0.033s latency).

PORT STATE SERVICE REASON VERSION
3000/tcp open http syn-ack ttl 63 Node.js Express framework
|_http-title: Site doesn't have a title (text/html; charset=utf-8).

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Mon Apr 30 11:31:19 2018 -- 1 IP address (1 host up) scanned in 13.24 seconds
[root:~/Desktop/celestial]#</pre>
I tried all manner of scans and this was the only port that I could find.

Browsing to the webpage gave me this.

<img class="alignnone size-full wp-image-98" src="http://offsecnewbie.com/wp-content/uploads/2018/05/firefox.png" alt="firefox" width="385" height="250" />

The source code didnt give me anything to go on. I decided to open burpsuite to see what i could find.

<img class="alignnone size-full wp-image-97" src="http://offsecnewbie.com/wp-content/uploads/2018/05/burp1.png" alt="burp1" width="1113" height="591" />

I noticed there was a cookie profile in the response. I wanted to see if i could decode this.

<img class="alignnone size-full wp-image-96" src="http://offsecnewbie.com/wp-content/uploads/2018/05/burpe2.png" alt="burpe2" width="1830" height="338" />

I did a URL decode followed by Base64 decode

<img class="alignnone size-full wp-image-100" src="http://offsecnewbie.com/wp-content/uploads/2018/05/burp4.png" alt="burp4" width="1883" height="582" />

As you can see it gave me something readable. At this point I'm not sure what to do with this new information.
