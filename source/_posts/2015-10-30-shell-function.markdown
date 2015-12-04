---
layout: post
title: Shell Function
date: 2015-10-30 22:34:34 -0500
comments: true
categories: bash
---

I have been using youtube-dl every week. The application is a "Small command-line program to download videos from YouTube.com and other video sites." It's super useful. However, it default downloads to the current terminal directory. Let's write a simple shell script in order to place all downloaded files from youtube-dl in the same directory.

```sh .zshrc
function mp3 {
  # Download all of the things to /Downloads

  youtube-dl --default-search=ytsearch: \
             --output="Downloads/%(title)s.%(ext)s" "$*"
}
```

The "$*" signifies the string following mp3, the example below, "$*" is the URL arument.

```sh
  mp3 https://www.youtube.com/watch\?v\=9fYeiGeuTFA
```

Now I can run the above script in order to download the video from the youtube URL.!

Helpful Links

[youtube-dl](https://github.com/rg3/youtube-dl)
