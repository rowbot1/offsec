---
layout: post
title: TryHackMe - Steel Mountain
date: 2019-10-12 23:07
author: rowbot
comments: true
categories: [TryHackThis, Uncategorized]
---
<!-- wp:paragraph {"align":"left"} -->
<p class="has-text-align-left">Use metasploit for initial access, utilise powershell for Windows privilege escalation enumeration and learn a new technique to get Administrator access.</p>
<!-- /wp:paragraph -->

<!-- wp:image {"align":"center","id":891,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image.png" alt="" class="wp-image-891"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":892,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-1.png" alt="" class="wp-image-892"/><figcaption>Autorecon scan reveal the following. I had to do another scan to pick up port 8080 for some reason autorecon missed it.</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":893,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-2.png" alt="" class="wp-image-893"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":894,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-3.png?fit=680%2C124&amp;ssl=1" alt="" class="wp-image-894"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":895,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-4.png?fit=680%2C197&amp;ssl=1" alt="" class="wp-image-895"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":896,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-5.png" alt="" class="wp-image-896"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":897,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-6.png" alt="" class="wp-image-897"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":898,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-7.png?fit=680%2C74&amp;ssl=1" alt="" class="wp-image-898"/><figcaption>added Inoke-AllChecks to bottom of PowerUp.ps1 file</figcaption></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":899,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-8.png?fit=680%2C158&amp;ssl=1" alt="" class="wp-image-899"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":900,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-9.png?fit=680%2C97&amp;ssl=1" alt="" class="wp-image-900"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":901,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-10.png" alt="" class="wp-image-901"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":902,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-11.png" alt="" class="wp-image-902"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":903,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-12.png" alt="" class="wp-image-903"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":904,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i2.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-13.png?fit=680%2C90&amp;ssl=1" alt="" class="wp-image-904"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":905,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://i0.wp.com/offsecnewbie.com/wp-content/uploads/2019/10/image-14.png?fit=680%2C76&amp;ssl=1" alt="" class="wp-image-905"/></figure></div>
<!-- /wp:image -->

<!-- wp:image {"align":"center","id":906,"sizeSlug":"large"} -->
<div class="wp-block-image"><figure class="aligncenter size-large"><img src="https://offsecnewbie.com/wp-content/uploads/2019/10/image-15.png" alt="" class="wp-image-906"/></figure></div>
<!-- /wp:image -->
