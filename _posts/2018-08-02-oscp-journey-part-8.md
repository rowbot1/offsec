---
layout: post
title: OSCP Journey Part 8
date: 2018-08-02 15:34
author: rowbot
comments: true
categories: [Progress Bar, Useful things]
---
<pre class="hljs css"><span class="hljs-selector-tag">Day:</span> <span class="hljs-selector-tag">-51</span>
<span class="hljs-selector-tag">PDF</span>: 90%
<span class="hljs-selector-tag">Videos</span>: 95%
<span class="hljs-selector-tag">Boxes</span>: 5
<span class="hljs-selector-tag">Networks</span><span class="hljs-selector-pseudo">:1</span></pre>
<p>Well I'm a bit more ubeat as I've put in a good amount of hours this week though would have liked to have done more. Work getting in the way of my learning! Anyways, I've just popped a Windows Box and it was a real struggle but finally got there in the end. Anyways just a blog while I've got the NT Authority\System horn. Here is some info I've used while popping this box;</p>
<p> </p>
<p>Good resource =<a href="http://www.fuzzysecurity.com/tutorials/16.html"> http://www.fuzzysecurity.com/tutorials/16.html </a></p>
<p>The difference between Staged and Non-Staged payloads:</p>
<p><strong>Staged = windows/shell/reverse_tcp</strong> = spawn a shell connect back to attacker<br />try to use this one more...i think.</p>
<p><strong>Non-Staged = windows/shell_reverse_tcp</strong> = connect back to attacker and spawn a shell<br />useful if bandwidth is an issue</p>
<p>Once you get logged in as a low priv shell find what hotfixes have been installed and see if they have any exploits</p>
<pre>systeminfo</pre>
<p>A useful command I've come across to pull info about the box + the proofs is:</p>
<pre>echo. &amp; echo. &amp; <span class="red">echo</span> whoami: &amp; <span class="red">whoami</span> 2&gt; nul &amp; <span class="red">echo</span> %username% 2&gt; nul &amp; echo. &amp; <span class="red">echo</span> Hostname: &amp; hostname &amp; echo. &amp; ipconfig /all &amp; echo. &amp; <span class="red">echo</span> proof.txt: &amp;  type <span class="green">"C:\Documents and Settings\Administrator\Desktop\proof.txt"</span></pre>
<p>Sometimes though the whoami or %hostname% won't work for some reason. If it doesn't do</p>
<pre>tasklist /v</pre>
<p>and get look for the user that your shell is logged in as.</p>
<p><span style="color: #ff0000;">Side note, always set FTP to binary mode by typing binary. This will stop anything you upload being corrupted, eg nc.exe.</span></p>
<p>Windows XP add admin user</p>
<pre>c:\&gt; net user /add rowbot rowbotpassword

c:\&gt; net localgroup administrators rowbot /add</pre>
<p>I need to learn more about post exploitation, as in quickly grab anything of interest. If anyone can point me to a resource that would be great.</p>
<p>Now to start the write up for this box.</p>

<!-- wp:image {"id":379,"align":"center"} -->
<figure class="wp-block-image aligncenter"><img src="http://offsecnewbie.com/wp-content/uploads/2018/08/happy-rowbot.png" alt="" class="wp-image-379"/></figure>
<!-- /wp:image -->
