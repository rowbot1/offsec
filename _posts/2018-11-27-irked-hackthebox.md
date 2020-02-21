---
layout: post
title: Irked - HacktheBox
date: 2018-11-27 12:36
author: rowbot
comments: true
categories: [Hackthebox, hackthebox, irked]
---
<!-- wp:paragraph -->
<p>I learned about SUID with this box. The user access I found easy, I think I got user in under 10 minutes - that's a first for me. The PE part took me sometime, which a few nudges!<br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Skills Required</strong><br>SUID knowledge<br><br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p><strong>Skills Learned</strong><br>Searching for sticky bits <br>Understanding a bit more about standard linux binaries<br>Adding echo command to a file to see if it executes it.<br><br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">START<br></pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>nmap -sC -sV -oA all -vv -p- 10.10.10.117</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":528} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-09-41-00.png" alt="" class="wp-image-528"/><figcaption>nmap imported into pentest.ws</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I see that there is an IRC server and take took a look for a metasploit module. <br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":493} -->
<figure class="wp-block-image"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-23-44.png?fit=680%2C139" alt="" class="wp-image-493"/><figcaption>metasploit search on pentest.ws</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":497} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-27-59.png" alt="" class="wp-image-497"/><figcaption>set RHOST and RPORT</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":500} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-29-53.png" alt="" class="wp-image-500"/><figcaption>have shell as ircd - need to upgrade shell.</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":501} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-30-47.png" alt="" class="wp-image-501"/><figcaption>I chose python 2&nbsp; and upgraded to a better shell</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After upgrading the shell I browsed to the home directory to see what users there were. I seen there was ircd - which I was logged in as and djmardov. Traversing into the djmardov user there were a few files - laid out in the Windows style<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">ircd@irked:/home/djmardov$ ls <br>ls <br>Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos<br>ircd@irked:/home/djmardov$ </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I ran ls -laR to search for files within all the directories in djmardov and found the user.txt file as well as another hidden file called .<em>backup</em>.<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":502} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-37-47.png" alt="" class="wp-image-502"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I tried to view user.txt but couldnt - I'm not the user djmardov.<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">ircd@irked:/home/djmardov/Documents$ cat user.txt<br>cat user.txt<br>cat: user.txt: Permission denied<br>ircd@irked:/home/djmardov/Documents$ </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I checked to see what was in the contents of .backup.txt<br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">ircd@irked:/home/djmardov/Documents$ cat .backup<br>cat .backup<br>Super elite steg backup pw<br>UPupDOWNdownLRlrBAbaSSss</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Humm so I have a steg password for something - probably an image. <br></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Back to review the nmap scan and see there is a http server. <br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":505} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-42-53.png" alt="" class="wp-image-505"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It reveals an image - I guessed that this is the image that the steg password is for.<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":506} -->
<figure class="wp-block-image"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-42-25.png?fit=680%2C471" alt="" class="wp-image-506"/><figcaption>image on homepage</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I downloaded the image to kali <br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":507} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-45-49.png" alt="" class="wp-image-507"/><figcaption>I notice that the file name is irked - adding to my suspicions that this is the image that i need to decode</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>For some reason steghide is not installed by default in Kali so I did an apt-get install steghide to install it. Following the help page of it I extracted the data from the file as shown below. <br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":508} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-50-03.png" alt="" class="wp-image-508"/><figcaption>captured another password - probably for the user djmardov</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":525} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-20-45-19.png" alt="" class="wp-image-525"/><figcaption>added creds to pentest.ws</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I ssh'd in as dj<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":509} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-16-51-39.png" alt="" class="wp-image-509"/></figure>
<!-- /wp:image -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">djmardov@irked:~$ cat Documents/user.txt <br>4a66a78b12dc0e661a59d3f5c0267a8e<br>djmardov@irked:~$ </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>The PE part took me a lot of time and I needed a few nudges. I'll go through some my thoughts, some commands I used and what I was looking for. <br></p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">djmardov@irked:/tmp$ sudo -l<br>-bash: sudo: command not found<br>djmardov@irked:/tmp$ </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Test to see what sudo command djmardov can run. He has not been given access to sudo. Sometimes you get lucky with this PE route.<br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":517} -->
<figure class="wp-block-image"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-20-14-42.png?fit=680%2C237" alt="" class="wp-image-517"/><figcaption>nothing of note running as root</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":518} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-20-30-12.png" alt="" class="wp-image-518"/><figcaption><code>What is the server's Architecture cat&nbsp;/proc/version;&nbsp;uname&nbsp;-a;&nbsp;uname&nbsp;-mrs;&nbsp;rpm&nbsp;-q&nbsp;kernel;&nbsp;dmesg&nbsp;|&nbsp;grep&nbsp;Linux;&nbsp;ls&nbsp;/boot&nbsp;|&nbsp;grep&nbsp;vmlinuz-;&nbsp;file&nbsp;/bin/ls;&nbsp;cat&nbsp;/etc/lsb-release</code></figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":519} -->
<figure class="wp-block-image"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-20-33-02.png?fit=680%2C258" alt="" class="wp-image-519"/><figcaption>Find apps installed;<br>                <code>ls&nbsp;-alh&nbsp;/usr/bin/;&nbsp;ls&nbsp;-alh&nbsp;/sbin/;&nbsp;dpkg&nbsp;-l;&nbsp;rpm&nbsp;-qa;&nbsp;ls&nbsp;-alh&nbsp;/var/cache/apt/archivesO;&nbsp;ls&nbsp;-alh&nbsp;/var/cache/yum/*;</code><br><br>nothing looks out of place here - checking versions of some apps.</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":522} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-20-35-53.png" alt="" class="wp-image-522"/><figcaption><code>Looking for&nbsp;</code>writeable config files - none found<br><code> find&nbsp;/etc/&nbsp;-writable&nbsp;-type&nbsp;f&nbsp;2&gt;/dev/null</code></figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":523} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-26-20-41-15.png" alt="" class="wp-image-523"/><figcaption>What Scheduled jobs? - None<br><code>crontab&nbsp;-l;&nbsp;ls&nbsp;-alh&nbsp;/var/spool/cron;&nbsp;ls&nbsp;-al&nbsp;/etc/&nbsp;|&nbsp;grep&nbsp;cron;&nbsp;ls&nbsp;-al&nbsp;/etc/cron*;&nbsp;cat&nbsp;/etc/cron*;&nbsp;cat&nbsp;/etc/at.allow;&nbsp;cat&nbsp;/etc/at.deny;&nbsp;cat&nbsp;/etc/cron.allow;&nbsp;cat&nbsp;/etc/cron.deny</code></figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":530} -->
<figure class="wp-block-image"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-09-55-03.png?fit=680%2C60" alt="" class="wp-image-530"/><figcaption>check to see if the system saves any history - maybe capture a password or username or interesting file in there - no luck<br><br><code>cat&nbsp;~/.bash_history;&nbsp;cat&nbsp;~/.nano_history;&nbsp;cat&nbsp;~/.atftp_history;&nbsp;cat&nbsp;~/.mysql_history;&nbsp;cat&nbsp;~/.php_history</code></figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":531} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/imageedit_2_2236803244.png" alt="" class="wp-image-531"/><figcaption>Look for binaries with the SUID or GUID bits set. <br><br><code>find&nbsp;/&nbsp;-perm&nbsp;-g=s&nbsp;-o&nbsp;-perm&nbsp;-4000&nbsp;!&nbsp;-type&nbsp;l&nbsp;-maxdepth&nbsp;6&nbsp;-exec&nbsp;ls&nbsp;-ld&nbsp;{}&nbsp;\;&nbsp;2&gt;/dev/null</code></figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>What I initially noticed was the date of /usr/bin/viewuser- it was 2018 meaning recently modified. I copied and pasted the other commands in to see what would happen. I also ran the command on my Kali machine <br></p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":534} -->
<figure class="wp-block-image"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-10-42-31.png?fit=680%2C328" alt="" class="wp-image-534"/><figcaption>I compared it to my Kali machine as it is Debian based. Again viewuser stood out so requires more investigation&nbsp;</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":535} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-19-47.png" alt="" class="wp-image-535"/><figcaption>running the binary says its looking for /tmp/listusers and can't find it.</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":536} -->
<figure class="wp-block-image"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-21-19.png?fit=680%2C177" alt="" class="wp-image-536"/><figcaption>contents of /tmp</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":537} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-22-29.png" alt="" class="wp-image-537"/><figcaption>I created listusers with 'echo this is a test' inside it and ran. Permission denied. Meaning it found the file but obviously doesn't have the permission to run it. I need to chmod it.</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":538} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-24-22.png" alt="" class="wp-image-538"/><figcaption>It worked! but what permissions does this file run as. Added whoami to the file</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":539} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-25-32.png" alt="" class="wp-image-539"/><figcaption>Success! It is running as root. Now to capture root.txt</figcaption></figure>
<!-- /wp:image -->

<!-- wp:image {"id":540} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-26-49.png" alt="" class="wp-image-540"/></figure>
<!-- /wp:image -->

<!-- wp:image {"id":541} -->
<figure class="wp-block-image"><img src="http://offsecnewbie.com/wp-content/uploads/2018/11/Screenshot-from-2018-11-27-11-27-00.png" alt="" class="wp-image-541"/><figcaption>root captured</figcaption></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>You could get that binary to execute a reverse shell with root access.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I learned some important stuff with this box. Check for sticky bits and look for binaries which are not standard in that version of linux. Also if your not sure what file is doing, add and echo&nbsp; command to see if it executes it. <br></p>
<!-- /wp:paragraph -->
