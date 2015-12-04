---
layout: post
title: "Git Commandments"
date: 2013-02-13 09:17
comments: true
categories: 
---

<div class="entry-content">
<p>Don&#8217;t change master.</p>
<p><code>$ git pull &#8211;&#8211;rebase</code></p>

<p>  -to pull from the master and then create new  branch</p>

<p>  -now you have an up to date branch to code your   magic</p>
<p><code>$ git status</code></p>

<p>  -run after you change things in your branch</p>

<p>  -shows you what changed</p>
<p><code>$ git add .</code></p>

<p><code>$ git commit -m &#8220;Your message&#8221;<code></p>

<p><code>$ git co master</code></p>

<p> -checkout master before your merge</p>

<p><code>$ git pull &#8211;&#8211;rebase</code></p>

<p>  -ensure that there haven&#8217;t been any changes before you merge to master branch</p>

<p><code>$ git merge <branch name> -m &#8220;Your message&#8221;</code></p>
<p><code>$ git push</code></p>

<p>-data now updated on git!</p>


</div>