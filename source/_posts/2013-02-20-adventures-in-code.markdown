---
layout: post
title: "Adventures in Code"
date: 2013-02-20 14:43
comments: true
categories: 
---

<div class="entry-content"><p>One can run into some cataclysmic errors trying to setup and deploy an Octopress account.  Personally, I kept raking new themes from Github to my Octopress account and finally, my blog broke.  In order to keep the same url, I deleted my Github repository and recreated an Octopress account.</p>

<h2>Things I learned</h2>

<p>Knowing:</p>

<p><code>  $ subl .</code></p>

<p>This saves so much time. The code snipped will open the entire current directory in your terminal.</p>

<p>Now, things are going well and I begin to play with some HTML, CSS and JS code.  I stumbled upon this gold nugget.  Sweet animated clouds <a href="http://www.clicktorelease.com/code/css3dclouds/">here.</a></p>

<p><img src = "/images/clouds.png" height = "600" width = "600"></p>

<p> ...and a tutorial to recreate animated clouds by the author <a href="http://www.clicktorelease.com/blog/how-to-make-clouds-with-css-3d">here.</a></p>

<p>It was loads of fun messing with the CSS and JS source code and I manipulated the code into this:</p>

<p><img src = "/images/smile.png" height = "600" width = "600"></p>

<p>Awesome! I created this a massive randomly generated semi-transparent smiley face.  Now, how do I implement this information to Octopress? The CSS ans JS code is hard core and I do not understand how works very well.  What do I do now?<p/>  

<a href="http://bonsaijs.org/">Bonsai JS</a> is a Javascript graphics library with step-by-step tutorials on how to implement all sorts of useful code such as rendering shapes, gradients and animation.  It also has a built in editor to play with said code.  With the editor and tutorials I can quickly learned how to do code graphics with JS.  An example of what can be created with Bonsai is below.</p>
</div>

<script src="http://cdnjs.cloudflare.com/ajax/libs/bonsai/0.4/bonsai.min.js"></script>
<div id="movie"></div>
<script>
  bonsai.run(document.getElementById('movie'), {
    code: function() {
      new Circle(150, 50, 30)
  .fill('red')
  .fill(gradient.radial(['rgba(0,0,0,0)', 'rgba(0,0,0,0.3)']))
  .addTo(stage)
  .animate('7s', { x: 500 }, { easing: 'bounceInOut', repeat: 100 })
    }
  });
</script>

<p>Sweet.  Thanks to my fellow Flatiron students who helped me along the way!</p>







