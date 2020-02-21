---
layout: post
title: Wizard Labs Browny
date: 2019-04-16 13:14
author: rowbot
comments: true
categories: [Uncategorized]
---
<!-- wp:image {"id":708,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-9.png" alt="" class="wp-image-708"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This was a very easy box, normally when a box is 1 or 2 out of 10 generally there is a metasploit module for it. It was the case for this machine.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>nmap -sC -sV -oA all -vv -p- 10.1.1.17</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":709,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-10.png" alt="" class="wp-image-709"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"id":710} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-11.png" alt="" class="wp-image-710"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>

Port 80 reveals a default website. My first thoughts are that the webserver is likely to have default settings like usernames and passwords.

</p>
<!-- /wp:paragraph -->

<!-- wp:html -->

<pre><span style="background-color:#2E3436"> </span><span style="background-color:#2E3436"><font color="#C4A000">⚡ </font></span><span style="background-color:#3465A4"><font color="#2E3436"> ~/Desktop/browny </font></span><font color="#3465A4"></font> <font color="#4E9A06">gobuster</font> -w <u style="text-decoration-style:single">/usr/share/wordlists/dirb/common.txt</u> -u http://10.1.1.17       

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://10.1.1.17/
[+] Threads      : 10
[+] Wordlist     : /usr/share/wordlists/dirb/common.txt
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2019/04/16 12:19:09 Starting gobuster
=====================================================
/.hta (Status: 403)
/.htaccess (Status: 403)
/.htpasswd (Status: 403)
/index.html (Status: 200)
/server-status (Status: 403)
=====================================================
2019/04/16 12:19:46 Finished
=====================================================
</pre>
<!-- /wp:html -->
